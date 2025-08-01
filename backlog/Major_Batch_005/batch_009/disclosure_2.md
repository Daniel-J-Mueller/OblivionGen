# 10943167

## Dynamic Granularity Processing Array

**Concept:** A processing array architecture that allows for dynamically reconfigurable processing element granularity, moving beyond fixed-size arrays. This enables optimization for varying workload characteristics within a single chip, and adapts to changing neural network layer requirements *during* execution.

**Specs:**

*   **Core Unit:** A base “Processing Core” consisting of a small number of arithmetic logic units (ALUs – say, 4). This is the fundamental building block.
*   **Interconnect:**  Each Processing Core has high-bandwidth, low-latency interconnects on all four sides (North, South, East, West). The interconnect is configurable to support different data transfer patterns (broadcast, point-to-point, reduction).
*   **Reconfiguration Hardware:** Each Processing Core includes local reconfiguration logic capable of physically connecting/disconnecting neighboring cores. This is achieved via micro-switches or similar technology.  Reconfiguration time is critical - target <10ns.
*   **Control Unit:** A central “Array Controller” responsible for:
    *   Monitoring workload characteristics (data volume, computational intensity).
    *   Determining optimal array granularity (number of active Processing Cores).
    *   Configuring the interconnect and reconfiguration hardware.
*   **Memory Hierarchy:**  Each Processing Core has a small, fast local memory.  A shared, larger memory is available for data staging and long-term storage.
*   **Scalability:** The array architecture should be scalable to hundreds or even thousands of Processing Cores.

**Operation:**

1.  **Initialization:** The Array Controller initializes the processing array with a default granularity (e.g., all cores active).
2.  **Workload Monitoring:** The Array Controller monitors the incoming workload characteristics.
3.  **Granularity Adjustment:** Based on the monitored characteristics, the Array Controller dynamically adjusts the granularity of the processing array:
    *   **High Computational Intensity, Low Data Volume:**  Reduce the number of active cores and increase the interconnect bandwidth to each core. This maximizes the computational throughput per core.
    *   **Low Computational Intensity, High Data Volume:** Increase the number of active cores and distribute the workload across them. This maximizes the overall throughput.
    *   **Variable Workload:**  Continuously adjust the granularity of the array to match the current workload requirements.
4.  **Reconfiguration:** The Array Controller configures the interconnect and reconfiguration hardware to reflect the new granularity.  This involves physically connecting/disconnecting neighboring cores and adjusting the data routing paths.
5.  **Data Processing:** The processing array processes the data according to the current configuration.

**Pseudocode (Array Controller):**

```
function adjust_granularity(workload_characteristics):
    if workload_characteristics.computational_intensity > threshold_high and \
       workload_characteristics.data_volume < threshold_low:
        reduce_granularity()
    elif workload_characteristics.computational_intensity < threshold_low and \
         workload_characteristics.data_volume > threshold_high:
        increase_granularity()
    else:
        maintain_granularity()

function reduce_granularity():
    for each inactive core in array:
        disconnect_core(core)
    reconfigure_interconnect()

function increase_granularity():
    for each disconnected core in array:
        connect_core(core)
    reconfigure_interconnect()

function reconfigure_interconnect():
    // Logic to update data routing paths based on the new array configuration
    pass
```

**Novelty:** This architecture differs from existing approaches by providing *dynamic* reconfiguration of the array granularity *during* execution.  Most existing systems use fixed-size arrays or limited reconfiguration capabilities. This design enables true adaptability to varying workload characteristics and allows for optimization across a wider range of applications. It is a hardware-centric approach to workload-aware computing.