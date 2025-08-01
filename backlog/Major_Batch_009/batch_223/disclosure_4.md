# 12014669

## Dynamic Scene Complexity Mapping for Per-Object Tone Adjustment

**Concept:** Extend the HDR/SDR comparison to not just *detect* candidate frames, but to generate a dynamic complexity map of the scene *per object*. This map will then drive individual tone and detail adjustments *within* the HDR frame before display, optimizing for both HDR fidelity and perceived detail on displays with limitations.

**Specs:**

*   **Input:** HDR video stream, SDR video stream (used as reference), output model data for various display types (edge-lit, backlit, OLED, etc.).
*   **Preprocessing:** Existing edge/object detection algorithm (from the patent) runs on both HDR and SDR frames, identifying and segmenting objects.
*   **Complexity Metric Generation:** For each object in the HDR frame:
    *   Calculate a "detail density" metric based on high-frequency component analysis (e.g., Laplacian variance) within the object's segmented area.
    *   Calculate a "dynamic range" metric within the object based on the difference between the maximum and minimum pixel values.
    *   Combine detail density and dynamic range into a single “Scene Complexity Score” (SCS) for the object. Weighting factors adjustable via UI.
*   **Display-Aware Adjustment:**
    *   Retrieve output model data for the detected display type. The model should include curves mapping SCS to acceptable detail/brightness levels.
    *   For each object:
        *   If SCS exceeds the display's threshold (based on the output model):
            *   Apply a localized tone mapping algorithm to reduce dynamic range *within the object* without affecting the rest of the frame.
            *   Apply localized sharpening/detail enhancement, intelligently balancing enhancement with noise introduction. Noise thresholds derived from the output model.
        *   If SCS is below the threshold, no adjustment is needed.
*   **Per-Frame Buffering/Processing:** Each frame is processed individually to adapt to scene changes.
*   **Output:** Adjusted HDR video stream optimized for the detected display type.

**Pseudocode:**

```
FOR each frame in HDR_video:
    FOR each object in detected_objects(HDR_frame):
        SCS = calculate_scene_complexity_score(object)
        display_model = get_display_model(detected_display_type)
        threshold = display_model.get_threshold(SCS)
        IF SCS > threshold:
            adjusted_object = apply_localized_tone_mapping(object, threshold)
            adjusted_object = apply_localized_sharpening(adjusted_object, display_model.noise_threshold)
            replace object in HDR_frame with adjusted_object
    output HDR_frame
```

**Hardware/Software Requirements:**

*   GPU acceleration for image processing.
*   Machine learning libraries for object detection and potentially for dynamic weighting factor adjustment.
*   Display detection API.
*   Configurable UI for adjusting weighting factors and model parameters.