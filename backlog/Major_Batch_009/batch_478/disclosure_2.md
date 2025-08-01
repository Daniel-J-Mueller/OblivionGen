# 11503341

## Adaptive Temporal Pixel Block Weighting for Perceptual Enhancement

**Specification:**

This system builds upon the described patent by introducing a dynamic weighting scheme for temporal pixel blocks during the DCT and inverse DCT stages. The aim is to enhance perceived video quality, particularly in scenes with high motion or frequent changes, by prioritizing more relevant temporal information.

**Core Concept:**

Instead of equally weighting pixel blocks from different frames (as implied in claim 2 & 6), a ‘relevance score’ is assigned to each temporal pixel block. This score dynamically adjusts based on motion estimation and perceptual salience analysis. Blocks with high motion or significant perceptual change receive higher weights.

**System Components:**

1.  **Motion Estimation Module:**
    *   Employs optical flow or block matching algorithms to estimate motion vectors between consecutive frames.
    *   Calculates a 'motion magnitude' for each pixel block based on the magnitude of its motion vectors.

2.  **Perceptual Salience Analysis Module:**
    *   Utilizes a saliency detection algorithm (e.g., based on contrast, color, or orientation changes) to identify perceptually salient regions within each frame.
    *   Assigns a 'salience score' to each pixel block reflecting the degree of visual attention it is likely to attract.

3.  **Relevance Score Calculation Module:**
    *   Combines the ‘motion magnitude’ and ‘salience score’ to generate a ‘relevance score’ for each temporal pixel block.
    *   Formula: `Relevance Score = α * Motion Magnitude + β * Salience Score`
        *   `α` and `β` are weighting parameters (tunable).

4.  **Weighted DCT & Inverse DCT:**
    *   During the DCT stage (as described in the patent), before restacking the DCT blocks, the DCT coefficients of each temporal pixel block are multiplied by its corresponding ‘relevance score’.
    *   During the inverse DCT stage, the same weighting is applied to the filtered DCT blocks before reconstructing the filtered pixel blocks.

**Pseudocode:**

```
// For each frame in the video
FOR each frame DO
    // Estimate motion vectors
    motion_vectors = estimate_motion(frame)

    // Calculate motion magnitude for each pixel block
    FOR each pixel_block IN frame DO
        motion_magnitude[pixel_block] = calculate_motion_magnitude(pixel_block, motion_vectors)
    END FOR

    // Detect salient regions
    salience_map = detect_salience(frame)

    // Calculate salience score for each pixel block
    FOR each pixel_block IN frame DO
        salience_score[pixel_block] = calculate_salience_score(pixel_block, salience_map)
    END FOR

    // Calculate relevance score for each temporal pixel block
    FOR each temporal_pixel_block DO
        relevance_score[temporal_pixel_block] = α * motion_magnitude[temporal_pixel_block] + β * salience_score[temporal_pixel_block]
    END FOR

    // Perform DCT on each temporal pixel block
    dct_blocks = perform_dct(temporal_pixel_blocks)

    // Apply relevance weighting to DCT coefficients
    FOR each dct_block, relevance_score IN zip(dct_blocks, relevance_scores) DO
        weighted_dct_block = dct_block * relevance_score
    END FOR

    // Perform wavelet transform and filtering (as in patent)
    filtered_dct_block = perform_wavelet_filtering(weighted_dct_block)

    // Perform inverse DCT
    filtered_pixel_block = perform_inverse_dct(filtered_dct_block)

    // Encode and transmit the filtered pixel block (as in patent)
END FOR
```

**Tunable Parameters:**

*   `α` and `β`: Weighting factors for motion and salience.
*   Parameters within the motion estimation and salience detection algorithms.
*   Wavelet filter parameters.

**Expected Benefits:**

*   Improved perceptual quality, particularly in dynamic scenes.
*   Enhanced sharpness and detail in areas of high motion.
*   Reduced blocking artifacts by emphasizing relevant information.