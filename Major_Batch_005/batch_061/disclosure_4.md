# 11210769

## Temporal Consistency with Learned Optical Flow & Style Transfer

**Core Concept:** Enhance video by not just upscaling *individual* frames, but leveraging learned optical flow to propagate style and detail from adjacent, higher-quality frames.  This moves beyond simple temporal filtering and focuses on stylistic consistency *and* detail injection.

**Specifications:**

**1. Optical Flow Network (OFN):**

*   **Input:**  Pairs of consecutive lower-resolution frames (Frame T, Frame T+1).
*   **Architecture:** A lightweight convolutional neural network (CNN) designed for dense optical flow estimation.  Focus on real-time performance, potentially utilizing a pyramid structure for multi-scale flow estimation.  Loss function combines endpoint error (L2 loss) with smoothness regularization.
*   **Output:** Dense optical flow field (vectors) representing the motion between Frame T and Frame T+1.

**2. Style & Detail Propagation Network (SDPN):**

*   **Input:**  
    *   Lower-resolution frame to be enhanced (Frame T).
    *   Higher-resolution 'reference' frame (Frame T-1 or T+1) - this could be a previously enhanced frame, or an original high-resolution frame if available.
    *   Optical flow field from OFN (describing motion from the reference frame to Frame T).
*   **Architecture:**  A U-Net style architecture with attention mechanisms.  Crucially, the network includes a ‘flow warping’ layer.
    *   **Flow Warping Layer:** Uses the optical flow field to warp features from the reference frame onto the current lower-resolution frame.  This creates a spatially aligned representation of the high-resolution details.  Bilinear or bicubic interpolation may be employed within this layer, but learned interpolation may be superior.
    *   **Style Transfer Module:**  Based on Gram matrix computation to capture stylistic characteristics of the reference frame. These stylistic features are then applied to the warped features to achieve stylistic consistency.
    *   **Detail Injection Module:**  Combines the warped and styled features with the features extracted from the lower-resolution input frame. This is done through a series of convolutional layers and residual connections to selectively inject high-resolution details without introducing artifacts.
*   **Output:** Enhanced higher-resolution frame.

**3. Temporal Smoothing Filter:**

*   **Input:** Sequence of enhanced frames.
*   **Algorithm:** A simple median filter or Gaussian filter applied along the temporal dimension to reduce flickering and improve overall smoothness. This is a post-processing step.
*   **Output:** Final, temporally smoothed, enhanced video.

**Pseudocode:**

```
FOR each frame T in video:
    // Optical Flow Estimation
    flow_field = OFN(frame T-1, frame T)

    // Style & Detail Propagation
    enhanced_frame = SDPN(frame T, frame T-1, flow_field)

    //Temporal Smoothing
    smoothed_frame = TemporalSmoothingFilter(enhanced_frame, previous_enhanced_frames)
    
    //Store Enhanced Frame
    previous_enhanced_frames.append(smoothed_frame)

END FOR
```

**Novelty:**

Existing methods often treat each frame independently or apply simple temporal filtering. This approach combines learned optical flow with style transfer and detail injection to create a more consistent and visually appealing result.  The explicit use of optical flow to warp and align features from adjacent frames allows for more accurate detail transfer and reduces artifacts.  The network’s architecture is tailored to exploit temporal information and achieve both stylistic consistency and high-resolution detail.