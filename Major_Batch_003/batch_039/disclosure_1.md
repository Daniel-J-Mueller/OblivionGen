# 11409838

## Dynamic Weight Granularity & Sparsity Engine

**Concept:** Exploit the observation that not all weight elements contribute equally to the convolution result. Instead of operating on entire weight matrices, dynamically adjust weight granularity (size of individual weight blocks processed) *and* introduce controlled sparsity based on real-time activation patterns. This minimizes data movement and computation, improving throughput.

**Specifications:**

**1. Weight Granularity Control Unit (WGCU):**

*   **Input:** Combined weight vector (as in the referenced patent), Granularity Configuration Register (GCR).
*   **Function:** Decomposes the combined weight vector into blocks of variable size defined by the GCR.  The GCR specifies block dimensions (e.g., 1x1, 2x2, 3x3, dynamic).
*   **Output:** Granular Weight Blocks (GWBs) – sets of weight elements representing a single block.

**2. Activation-Dependent Sparsity Engine (ADSE):**

*   **Input:** GWBs from the WGCU, Data Matrix elements from Data Input Vector Unit, Sparsity Threshold Register (STR).
*   **Function:**  For each GWB:
    *   Calculate the expected activation contribution. This can be approximated by multiplying the input data elements that will interact with this GWB with corresponding elements of the GWB.
    *   Compare the calculated contribution with the STR.
    *   If the contribution is below the STR, zero out the GWB (create a sparse block).
*   **Output:** Sparse GWBs (SGWBs) – weight blocks with potentially zeroed-out elements.

**3. Dynamic Routing Network (DRN):**

*   **Input:** SGWBs from ADSE, Data Matrix elements from Data Input Vector Unit.
*   **Function:** Route SGWBs to appropriate calculation units based on data dependencies and block location.  Zeroed-out blocks are bypassed entirely.
*   **Output:** Active Weight Blocks (AWBs) – SGWBs that are not bypassed.

**4. Calculation Unit Adaptation:**

*   Calculation units are modified to handle variable-sized AWBs.  This could involve reconfigurable multiply-accumulate (MAC) units or specialized hardware for processing sparse data.

**Pseudocode (ADSE):**

```
function apply_sparsity(GWB, DataElements, STR) {
  activation_contribution = 0
  for each element in GWB {
    corresponding_data_element = get_corresponding_element(DataElements, GWB) // Mapping to the correct data element
    activation_contribution += element * corresponding_data_element
  }

  if (activation_contribution < STR) {
    // Zero out the GWB
    for each element in GWB {
      element = 0
    }
  }
  return GWB
}
```

**Registers:**

*   **GCR (Granularity Configuration Register):** Defines the size of the weight blocks to be processed.
*   **STR (Sparsity Threshold Register):** Sets the threshold for determining whether a weight block should be zeroed out.
*   **Data Dependency Map:** A map detailing the relationship between data matrix elements and weight blocks for efficient routing.

**Potential Benefits:**

*   Reduced data movement: Fewer weight elements need to be transferred between memory and calculation units.
*   Improved throughput: Zeroed-out weight blocks bypass computation, allowing calculation units to focus on active blocks.
*   Dynamic adaptation: The system can adapt to different data patterns and network architectures.
*   Potential for significant energy savings.