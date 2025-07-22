# 10244266

## Adaptive Noise Sculpting via Perceptual Masking

**Concept:** Extend the noise reduction techniques by integrating a perceptual masking model to dynamically adjust noise removal intensity based on human visual sensitivity. This moves beyond simple thresholding and quantization by prioritizing noise reduction where it *matters* most to the viewer, while preserving detail in perceptually sensitive areas.

**Specs:**

1.  **Perceptual Masking Model Integration:**
    *   Implement a Just Noticeable Difference (JND) model based on the Discrete Cosine Transform (DCT) coefficients. This will calculate the threshold below which noise will be imperceptible.
    *   The JND model will be influenced by:
        *   Local image contrast.
        *   Luminance levels.
        *   Spatial frequency (higher frequencies are more susceptible to masking).
        *   Viewer characteristics (optional – adjustable parameters for different visual sensitivities).

2.  **Adaptive Noise Thresholding:**
    *   For each Coding Unit (CU), calculate a masking threshold map using the JND model.
    *   Apply this map to the high-frequency DCT coefficients *before* the existing isolated coefficient removal and quantization steps.
    *   Isolated coefficients below the masking threshold are removed (set to zero).
    *   Non-isolated coefficients are quantized *adaptively* based on the masking threshold. Coefficients near the threshold receive lower quantization levels, preserving more detail.

3.  **Noise Sculpting Function:**
    *   Introduce a ‘Noise Sculpting Function’ that modulates the quantization matrix. This function takes the masking threshold map and the original quantization matrix as input.
    *   The function calculates a modified quantization matrix where values are scaled proportionally to the masking threshold.
    *   Areas with high masking thresholds (where noise is less visible) receive higher quantization levels (more aggressive compression).
    *   Areas with low masking thresholds receive lower quantization levels (less aggressive compression, preserving detail).

4.  **CU-Level Noise Profiling:**
    *   Analyze each CU to determine its noise characteristics (using statistical measures on the residual coefficients).
    *   Classify CUs into ‘high-noise’, ‘medium-noise’, and ‘low-noise’ categories.
    *   Apply different Noise Sculpting Functions or parameter sets to each category.

5. **Pseudocode:**

```
FOR each CU in Frame:
    residual_matrix = calculate_residual_matrix(CU)
    frequency_domain_matrix = DCT(residual_matrix)
    masking_threshold_map = calculate_JND_map(frequency_domain_matrix)

    FOR each DCT coefficient in frequency_domain_matrix:
        IF coefficient is isolated AND abs(coefficient) < masking_threshold_map[coefficient_index]:
            coefficient = 0

        IF coefficient is not isolated:
            quantization_level = original_quantization_level * (masking_threshold_map[coefficient_index] / base_threshold) #scale quantization
            quantized_coefficient = quantize(coefficient, quantization_level)

    reconstructed_residual = inverse_DCT(quantized_coefficients)
    encoded_CU = encode(reconstructed_residual)
```

6.  **Hardware Acceleration Considerations:**
    *   The JND calculation and Noise Sculpting Function can be parallelized efficiently on GPUs or dedicated hardware accelerators.
    *   Optimized lookup tables for the JND calculation can reduce computational cost.
    *   Hardware support for variable quantization levels can improve performance.