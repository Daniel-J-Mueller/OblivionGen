# 10812550

## Dynamic Complexity Masking & Per-Object Bitrate Allocation

**Specification:** A system for dynamically generating complexity masks for individual video frames within a multichannel stream, coupled with per-object bitrate allocation.

**Core Innovation:** The existing patent focuses on frame-level complexity. This expands on that by analyzing *within*-frame complexity, identifying individual objects, and allocating bitrate based on object characteristics *and* motion, creating a drastically more nuanced and efficient bitrate distribution.

**System Components:**

1.  **Object Segmentation Module:** Utilizing a deep learning-based object segmentation network (e.g., Mask R-CNN, DETR) to identify and segment all discernible objects within each frame of each channel. Output: A mask for each object, indicating its pixel boundaries.

2.  **Complexity Mask Generator:** This module creates a ‘complexity mask’ for each object, factoring in:
    *   **Texture Complexity:** Measure of high-frequency detail (variance of gradients).
    *   **Motion Vector Analysis:** Compute dense optical flow and derive a ‘motion magnitude’ map for each object. Higher motion = higher complexity.
    *   **Object Class:** Certain object classes (e.g., foliage, fire) are inherently more visually complex and require higher bitrate.
    *   **Occlusion Level:** Objects partially occluded require less detail on the obscured parts.

    The Complexity Mask is a per-pixel value representing the object's visual complexity, ranging from 0 (low) to 1 (high).

3.  **Bitrate Allocation Engine:** This is the central control.
    *   **Global Aggregate Bitrate:** The system still operates within the maximum aggregate bitrate constraint.
    *   **Channel-Specific Minimum/Maximum Bitrates:** As defined by the existing patent.
    *   **Object-Level Bitrate Budget:** The Engine divides the bitrate budget for each channel among the detected objects. The budget assigned to each object is proportional to:
        *   The object's area (number of pixels).
        *   The average complexity value from the Complexity Mask.
        *   The average motion vector magnitude.
        *   A ‘priority’ factor based on object class (e.g., prioritize faces, text).

4.  **Encoding Pipeline:**
    *   Each object is encoded separately using a video codec (e.g., HEVC, AV1).
    *   Quantization Parameter (QP) selection for each object is determined by its allocated bitrate budget. Lower QP = higher quality, but requires more bitrate.
    *   Objects are seamlessly recomposed into the final video stream.

**Pseudocode (Bitrate Allocation Engine):**

```
function allocate_bitrate(channel, objects, max_bitrate, min_bitrate, avg_bitrate):
  total_area = sum(object.area for object in objects)
  channel_bitrate = max(min(max_bitrate, avg_bitrate), min_bitrate) # Respect channel constraints

  remaining_bitrate = channel_bitrate

  for object in objects:
    complexity = average(object.complexity_mask)
    motion = average(object.motion_vectors)
    priority = object.priority_factor # Scale from 0 to 1
    object_score = complexity * motion * priority
    object_budget = (object_score / sum(object_score for object in objects)) * remaining_bitrate

    object_budget = max(min_bitrate/total_area * object.area, object_budget) #Ensure minimum bitrate

    remaining_bitrate -= object_budget
    object.bitrate = object_budget

  return objects
```

**Novelty:** This goes beyond frame-level complexity to provide *dynamic*, *object-specific* bitrate allocation. By prioritizing visually complex objects and allocating bitrate accordingly, the system can achieve higher perceived quality at a lower overall bitrate, particularly in scenes with varied content. The object-level approach enables a more granular and efficient use of the available bandwidth. The system could even pre-encode common objects into a 'library' to reduce on-the-fly encoding time.