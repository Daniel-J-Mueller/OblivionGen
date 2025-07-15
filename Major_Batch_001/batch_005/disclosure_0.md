# 10002644

## Dynamic Frame-Weighting for Perceptual Streaming

**Concept:** Extend the selective frame dropping/retention described in the patent to dynamically adjust *weights* applied to retained frames during encoding and decoding, based on real-time perceptual analysis of the video content. This creates a system optimized for perceived quality *at varying bandwidths*, rather than simply maintaining temporal coherence.

**Specs:**

**1. Perceptual Analysis Module (PAM):**

*   **Input:** Raw video frames.
*   **Process:**
    *   **Motion Vector Analysis:** Extract motion vectors to determine areas of high and low motion.
    *   **Scene Change Detection:** Identify abrupt scene changes, fades, and dissolves.
    *   **Saliency Mapping:** Generate a saliency map highlighting visually important regions.  This can utilize existing algorithms (e.g., Itti, Koch, and Niebur) or more advanced deep learning models.
    *   **Color Histogram Analysis:**  Analyze color distributions to identify regions with high visual complexity.
*   **Output:** Per-frame “Perceptual Importance Score” (PIS). This score is a weighted sum of the motion vector magnitude, scene change indicator, saliency map intensity (averaged over the frame), and color complexity (variance). Weights are tunable parameters.

**2. Dynamic Frame-Weighting Encoder:**

*   **Input:** Raw video frames, PIS from PAM.
*   **Process:**
    *   **Frame Categorization:** Categorize each frame as:
        *   *High Importance*: PIS > Threshold_High
        *   *Medium Importance*: Threshold_Low < PIS < Threshold_High
        *   *Low Importance*: PIS < Threshold_Low
    *   **Quantization Parameter Adjustment:** Adjust the quantization parameter (QP) for each frame *during encoding* based on its category:
        *   High Importance: Lowest QP (highest quality)
        *   Medium Importance: Intermediate QP
        *   Low Importance: Highest QP (lowest quality)
    *   **Bitrate Control:** Implement a closed-loop bitrate control system that dynamically adjusts the overall QP (and therefore bitrate) based on the desired target bitrate and the number of high/medium/low importance frames.
    *   **Metadata Insertion:** Insert metadata into the encoded stream indicating the “importance category” of each frame.

**3. Dynamic Frame-Weighting Decoder:**

*   **Input:** Encoded video stream with metadata.
*   **Process:**
    *   **Metadata Extraction:** Extract the importance category metadata for each frame.
    *   **Frame Prioritization:** When bandwidth is limited (e.g., network congestion):
        *   Prioritize transmission/decoding of High Importance frames.
        *   Drop or severely reduce the quality of Low Importance frames.
        *   Apply progressive refinement techniques to Medium Importance frames.
    *   **Perceptual Enhancement:** Apply post-processing techniques to enhance the perceptual quality of retained frames. This could include:
        *   Sharpening filters to emphasize important details.
        *   Color correction to improve visual appeal.

**Pseudocode (Decoder - Bandwidth Limitation):**

```
function decode_frame(encoded_frame, metadata):
  importance = metadata.importance
  if network_bandwidth < target_bandwidth:
    if importance == "Low":
      drop_frame()
      return
    elif importance == "Medium":
      decode_frame_with_reduced_quality()
      return
  decode_frame_with_full_quality()
```

**Tunable Parameters:**

*   `Threshold_High`, `Threshold_Low`:  Values determining PIS categorization.
*   QP adjustment multipliers for each importance level.
*   Weights for each component of the PIS calculation (motion vectors, scene changes, saliency, color complexity).
*   Target Bitrate.