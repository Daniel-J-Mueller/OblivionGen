# 11930222

## Dynamic Film Grain Reconstruction & Layered Encoding

**Concept:** Instead of *removing* film grain for compression, actively *reconstruct* it as a separate, highly compressed layer, allowing for adjustable grain re-introduction on the decoding side. This leverages the perceptual characteristics of film grain – often less critical for core image detail – to maximize compression efficiency while maintaining aesthetic control.

**Specs:**

**1. Grain Isolation Module (GIM):**

*   **Input:** Raw video frame.
*   **Process:**
    *   Decompose the input frame into a “base layer” and a “grain layer” using a learned decomposition network (e.g., a U-Net variant). The network is trained to separate perceptually significant details from stochastic, high-frequency noise (film grain).
    *   The decomposition network outputs:
        *   *Base Layer:*  A low-frequency representation of the core image content.
        *   *Grain Map:* A high-frequency representation containing primarily film grain and noise.  The network should be trained to *emphasize* grain patterns in this map, even at the cost of some detail loss.
*   **Output:**  Base Layer (image), Grain Map (image).

**2. Base Layer Encoding:**

*   **Input:** Base Layer (image).
*   **Process:** Encode the Base Layer using a modern video codec (e.g., VVC, AV1) with standard parameters.  Prioritize higher quality encoding for this layer, as it contains the core visual information.
*   **Output:** Encoded Base Layer bitstream.

**3. Grain Map Analysis & Vectorization:**

*   **Input:** Grain Map (image).
*   **Process:**
    *   Divide the Grain Map into adaptive-sized blocks.
    *   For each block:
        *   Calculate a texture vector representing the statistical characteristics of the grain (e.g., variance, contrast, spatial frequency, directionality).
        *   Quantize the texture vector.
        *   Store the quantized texture vector along with an index representing the block’s location.
*   **Output:**  Quantized texture vector stream, block index stream.

**4. Grain Map Compression:**

*   **Input:** Quantized texture vector stream, block index stream.
*   **Process:** Compress the texture vectors using a lossless or near-lossless compression algorithm optimized for statistical data (e.g., arithmetic coding, Huffman coding).  Pack the compressed texture vectors and block indices into a bitstream.
*   **Output:** Compressed Grain Map bitstream.

**5. Encoded Output:**

*   **Structure:** A container format containing the Encoded Base Layer bitstream and the Compressed Grain Map bitstream, along with metadata describing the encoding parameters.

**Decoding Process:**

1.  Decode the Encoded Base Layer bitstream to obtain the Base Layer image.
2.  Decode the Compressed Grain Map bitstream to obtain the texture vectors and block indices.
3.  For each block:
    *   Reconstruct the Grain Map block based on the corresponding texture vector.
    *   Add the reconstructed Grain Map block to the Base Layer image.
4.  Output the reconstructed video frame.



**Novelty:** The system doesn't *remove* grain, but isolates and compresses it as a separate stream, allowing for adjustable grain re-introduction. This opens possibilities for post-processing, stylistic control, and potential bandwidth savings, particularly in scenarios where grain is visually important. Furthermore, the learned decomposition network creates a uniquely adaptive process.