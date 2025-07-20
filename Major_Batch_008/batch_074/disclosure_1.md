# 11503341

## Adaptive Temporal Blending with Perceptual Anchors

**Concept:** Extend the temporal redundancy reduction by introducing a dynamically weighted blending of frames, guided by perceptually significant features (anchors). Instead of simply encoding differences between frames, the system *identifies* key visual elements – edges, textures, motion vectors – and prioritizes their preservation across frames through a learned blending process.

**Specs:**

1.  **Perceptual Anchor Detection Module:**
    *   Input: Video Frame
    *   Process: Employ a lightweight CNN (e.g., MobileNet variant) pre-trained on a salient object/edge detection dataset. Outputs a ‘perceptual map’ highlighting regions of high visual importance. This map indicates areas where *change* should be minimized to maintain perceived quality.  The map is represented as a probability distribution.
    *   Output: Perceptual Map (2D array of probabilities)

2.  **Temporal Motion Estimation & Vector Field Generation:**
    *   Input: Current Frame, Previous Frame
    *   Process: Utilize a block-matching algorithm (e.g., Enhanced Deposit-Matching Algorithm - EDMA) or optical flow to estimate motion vectors for each block in the current frame, referencing the previous frame.
    *   Output: Motion Vector Field (2D array of vectors)

3.  **Adaptive Blending Weight Calculation:**
    *   Input: Perceptual Map, Motion Vector Field
    *   Process:
        *   **Motion-Weighted Perceptual Influence:**  For each block, scale the perceptual map value by the *magnitude* of the corresponding motion vector.  High motion areas *reduce* the reliance on the perceptual map, prioritizing motion clarity.  Low motion areas increase the reliance on maintaining visual fidelity.
        *   **Temporal Consistency Score:** Calculate a consistency score for each block based on the similarity between the current block and the corresponding block in the previous frame (e.g., Sum of Absolute Differences - SAD).
        *   **Blending Weight Formula:**
            `Weight = α * (1 - SAD_normalized) + β * (1 - Motion_Magnitude_normalized) + γ * Perceptual_Influence`
            where α, β, and γ are tunable parameters controlling the balance between temporal consistency, motion clarity, and perceptual fidelity. SAD_normalized and Motion_Magnitude_normalized scale the values to a 0-1 range.
    *   Output: Blending Weight Map (2D array of weights, 0-1)

4.  **Frame Blending & Encoding Module:**
    *   Input: Current Frame, Previous Frame, Blending Weight Map
    *   Process:  Perform a weighted average of the current and previous frames, using the blending weight map to determine the contribution of each frame for each block.
        `Blended_Block = Weight * Current_Block + (1 - Weight) * Previous_Block`
    *   Output: Blended Frame
    *   Encoding: Standard video encoding (e.g., H.265) of the blended frame.

**Pseudocode:**

```
FOR each block in frame:
    motion_vector = estimate_motion(current_block, previous_block)
    perceptual_influence = perceptual_map[block_x, block_y]
    sad = calculate_sad(current_block, previous_block)
    weight = α * (1 - normalize(sad)) + β * (1 - normalize(motion_vector_magnitude)) + γ * perceptual_influence
    blended_block = weight * current_block + (1 - weight) * previous_block

encode(blended_frame)
```

**Novelty:** This system moves beyond simple motion compensation and introduces a *perceptually driven* blending process. By weighting frames based on visual significance and motion, it can dynamically prioritize either maintaining visual fidelity or preserving motion clarity, potentially leading to better perceived quality at lower bitrates.  The dynamic weighting adjusts based on the content, maximizing compression where perceptual impact is minimal.