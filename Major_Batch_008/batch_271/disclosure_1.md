# 11290710

## Adaptive Prediction with Contextual Entropy Modulation

**Concept:** This system expands upon the idea of variable-length coding by introducing *prediction* of pixel values, followed by encoding the *residual* (difference between predicted and actual value). Critically, the entropy coding (like Golomb) is *dynamically adjusted* based on the local image context.

**Motivation:** Traditional Golomb coding assumes a geometric distribution of input values. Real images aren't geometrically distributed; certain areas have more predictable values (smooth gradients), while others are highly textured and less predictable.  Adapting the entropy coding to match the local image characteristics will yield higher compression.

**Specs:**

**1. Prediction Engine:**

*   **Block-Based:** Image divided into variable-sized blocks (e.g., 4x4, 8x8, dynamically sized).
*   **Multi-Modal Prediction:**  Each block has multiple predictors:
    *   **DC Prediction:** Average of neighboring block’s DC component.
    *   **Median Prediction:** Median of neighboring pixel values.
    *   **Gradient Prediction:**  Estimate gradient from neighboring blocks and extrapolate.
    *   **Learned Prediction:**  A small neural network (trained offline or online) predicts the block's values based on surrounding blocks. (Consider a convolutional autoencoder).
*   **Predictor Selection:** A cost function (e.g., Mean Squared Error) determines the best predictor for each block. The index of the selected predictor is encoded alongside the residual data.

**2. Residual Calculation:**

*   `Residual = Actual Pixel Value - Predicted Pixel Value`.

**3. Adaptive Entropy Encoding:**

*   **Context Modeling:** For each residual value:
    *   Determine the local context:  Neighboring pixel values, block edge characteristics (e.g., strong edge, smooth transition).
    *   Use a context tree to map the context to a probability model.
*   **Dynamic Golomb Parameter Adjustment:**
    *   The probability model is used to estimate the geometric parameter (k) for the Golomb code.  Higher probability -> lower k, lower probability -> higher k.
    *   A lookup table maps context to a range of acceptable 'k' values.  Fine-grained 'k' adjustment provides better adaptation.
*   **Encoding:**  Golomb code the residual value using the dynamically adjusted 'k'.

**4. Bitstream Format:**

*   **Header:** Image dimensions, block size information.
*   **Data:** For each block:
    *   Selected Predictor Index (e.g., 0-3).
    *   Encoded Residual Data (Golomb codes).

**Pseudocode (Encoding):**

```
function encode_image(image):
  header = create_header(image)
  bitstream = header

  for each block in image:
    best_predictor = select_best_predictor(block, neighboring_blocks)
    bitstream += encode_predictor_index(best_predictor)

    residuals = calculate_residuals(block, best_predictor)
    for each residual in residuals:
      context = get_context(residual, neighboring_pixels)
      k = lookup_golomb_k(context) #Context to Golomb Parameter Lookup
      golomb_code = encode_golomb(residual, k)
      bitstream += golomb_code

  return bitstream
```

**Potential Enhancements:**

*   **Vector Quantization (VQ):**  Group residuals into vectors and use VQ before entropy coding.
*   **Rate-Distortion Optimization:**  Jointly optimize predictor selection and 'k' adjustment to minimize rate and distortion.
*   **Online Adaptation:** Update the context models and lookup tables dynamically during encoding/decoding.