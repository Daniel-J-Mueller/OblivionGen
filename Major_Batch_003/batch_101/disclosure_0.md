# 11699081

## Dynamic Kernel Partitioning for Distributed Convolution

**Concept:** Extend the local padding generation to a distributed system by dynamically partitioning the filter kernel itself and distributing sub-kernels to multiple hardware accelerators. This minimizes data movement *and* computation per accelerator, allowing for significantly larger kernels and/or activation datasets.

**Specifications:**

1.  **Kernel Partitioning Module (Software – runs on physical processor):**
    *   Input: Filter kernel, activation data set dimensions, number of available hardware accelerators.
    *   Function:  Divides the filter kernel into N sub-kernels (where N = number of hardware accelerators).  Partitioning strategy prioritizes minimizing inter-accelerator communication.  Example:  For a 2D kernel, partitioning could be horizontal or vertical stripes. More complex strategies consider kernel weights and activation data characteristics.
    *   Output:  List of sub-kernels, mapping of sub-kernel to hardware accelerator ID, communication map outlining data dependencies between sub-kernels.

2.  **Distributed Convolution Engine (Hardware – Implemented on each accelerator):**
    *   Input:  Sub-kernel, portion of activation data set, communication map, hardware accelerator ID.
    *   Function:
        *   Receives its assigned sub-kernel and corresponding activation data portion.
        *   Performs partial convolution using its sub-kernel and data.
        *   **Padding Generation:**  Similar to the original patent, generates padding data *only* for the portion of activation data it processes, based on boundary conditions.  Crucially, the padding generated is relevant to *its* sub-kernel.
        *   **Communication:**  Sends intermediate results (partial convolutions) and boundary padding data to other accelerators as dictated by the communication map.
        *   **Aggregation:** Receives intermediate results and padding from other accelerators.

3.  **Inter-Accelerator Communication Protocol:**
    *   High-bandwidth, low-latency communication protocol (e.g., NVLink, InfiniBand).
    *   Data format standardization for intermediate results and padding data.
    *   Error detection and correction mechanisms.

4.  **Aggregation Module (Software – runs on physical processor):**
    *   Input: Intermediate results and padding data from all accelerators.
    *   Function: Aggregates the partial convolutions into the final output data set.

**Pseudocode (Aggregation Module):**

```
function aggregate_results(partial_convolutions, padding_data):
    output_data = initialize_output_data(dimensions)
    for each partial_convolution in partial_convolutions:
        insert_partial_convolution(output_data, partial_convolution)
    for each padding_data_block in padding_data:
        insert_padding_data(output_data, padding_data_block)
    return output_data
```

**Novelty:**

This design moves beyond simply generating padding *within* a single accelerator to enabling convolution with kernels that are larger than the memory capacity of a single accelerator.  The dynamic partitioning and communication allow for scalability and increased computational power.  It’s not about faster convolution on a single device, but larger, more complex convolutions *possible* through distribution.