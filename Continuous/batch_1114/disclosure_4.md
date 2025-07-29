# 10990650

## Dynamic Padding Masking & Selective Computation

**Concept:** Expand on the idea of avoiding computations with padding by *dynamically* shaping a masking function applied *during* the matrix multiplication itself, allowing for irregularly shaped or ‘sparse’ padding regions – and leveraging this for specialized hardware acceleration.

**Specification:**

**1. Hardware Component: Adaptive Mask Generator (AMG)**

*   **Input:**
    *   Input Feature Map Dimensions (Height, Width).
    *   Weight Matrix Dimensions (Kernel Height, Kernel Width).
    *   Padding Size (number of padding elements on each side).
    *   Padding Configuration: A bitmask defining *which* padding regions are active.  This allows for non-uniform padding (e.g. more padding on the top/left than the bottom/right, or even only padding specific corners).  This bitmask would be a small lookup table –  e.g. 4x4 for defining padding status on a 2x2 kernel.
*   **Output:** A dynamically generated mask array, the same dimensions as the output feature map element being calculated. The mask contains 0s where computation should be skipped (padding regions) and 1s where computation should proceed.

**2. Computation Engine Modification:**

*   The standard matrix multiplication unit is augmented with a masking input.
*   Before each element-wise multiplication (input element * weight element), the corresponding element in the mask is checked.
*   If the mask element is 0, the multiplication is skipped, and the output is set to 0 (or another predefined value).
*   If the mask element is 1, the multiplication proceeds normally.

**3. Software/Control Flow:**

1.  **Pre-processing:** The host processor determines the padding configuration based on the input data and desired output.
2.  **AMG Configuration:** The host processor sends the padding configuration, input/weight dimensions to the AMG.
3.  **Dynamic Mask Generation:** The AMG generates the mask array for the current output feature map element.
4.  **Masked Matrix Multiplication:** The computation engine performs the matrix multiplication, utilizing the dynamically generated mask.
5.  **Repeat:** Steps 3-4 are repeated for each output feature map element.

**Pseudocode (AMG core logic):**

```pseudocode
function generate_mask(input_height, input_width, kernel_height, kernel_width, padding_size, padding_config):
  mask = array of size (input_height, input_width) initialized to 1
  for i in range(input_height):
    for j in range(input_width):
      if (i < padding_size or i >= input_height - padding_size or
          j < padding_size or j >= input_width - padding_size):

        #Check Padding Configuration
        if (padding_config[i % kernel_height][j % kernel_width] == 0):
            mask[i][j] = 0
  return mask
```

**Potential Benefits:**

*   **Increased Flexibility:** Handles irregular/sparse padding, opening possibilities for advanced data augmentation.
*   **Hardware Acceleration:** The AMG is designed for parallel execution, creating potential for specialized hardware implementation.
*   **Reduced Computation:**  Avoids unnecessary multiplications, saving power and improving performance.

**Additional Considerations:**

*   AMG memory access patterns need to be optimized for efficient mask generation.
*   Explore different padding configurations and their impact on performance.
*   Investigate methods for compressing the padding configuration to reduce communication overhead.