# 9766462

## Dynamic Focal Plane HMD with Micro-Lens Array & Eye-Tracking

**Concept:** Enhance the HMD experience by dynamically adjusting the focal plane of each eye *independently* using a micro-lens array combined with precise eye-tracking. This mitigates eye strain and improves perceived image quality, particularly for near/far transitions. It also opens the door to selectively blurring portions of the display to direct user attention.

**Specifications:**

*   **Display Stack:**
    *   Dual Micro-OLED displays (one per eye) – high resolution, fast response.
    *   Micro-Lens Array (MLA) – densely packed, individually controllable lenses positioned *above* each Micro-OLED. Lens pitch approximately 0.5mm. MLA fabrication using nano-imprint lithography.
    *   Polarizing layer between MLA and user’s eye.
*   **Eye-Tracking System:**
    *   Integrated infrared (IR) illumination and camera system. Minimum sampling rate 120Hz.
    *   Pupil center and gaze direction tracking algorithms. Accuracy: <0.5 degrees of visual angle.
    *   Real-time calibration procedure.
*   **Control System:**
    *   Dedicated image processing unit (IPU).
    *   Algorithm to determine optimal lens curvature for each eye based on:
        *   Gaze direction.
        *   Displayed content depth map.
        *   User’s IPD (interpupillary distance).
        *   Predefined comfort profile (adjustable by user).
    *   Each micro-lens controlled by an individual micro-actuator (e.g., MEMS).
*   **Software/Firmware:**
    *   SDK for developers to integrate depth data into content.
    *   API to access eye-tracking data.
    *   User interface to adjust comfort profile and calibration.
    *   Algorithm for selective blurring - based on user attention and potentially integrated with AR/VR applications.

**Pseudocode - Core Control Loop (IPU):**

```
loop:
    // Get eye-tracking data for left and right eye
    left_eye_gaze = get_eye_tracking_data(LEFT_EYE)
    right_eye_gaze = get_eye_tracking_data(RIGHT_EYE)

    // Get depth map of displayed content
    depth_map = get_depth_map()

    // Calculate optimal lens curvature for each eye
    left_lens_curvature = calculate_lens_curvature(left_eye_gaze, depth_map)
    right_lens_curvature = calculate_lens_curvature(right_eye_gaze, depth_map)

    // Send control signals to micro-actuators
    set_lens_curvature(LEFT_EYE, left_lens_curvature)
    set_lens_curvature(RIGHT_EYE, right_lens_curvature)

    // Repeat
```

**Potential Extensions:**

*   **Foveated Rendering Enhancement:** Combine dynamic focal plane with foveated rendering, focusing rendering resources on the area the user is *actively* looking at while subtly blurring peripheral content.
*   **Attention Guidance:** Developers could use the eye-tracking data and blurring capabilities to subtly direct user attention to specific elements within the scene.
*   **Accommodation Training:** Use the system to provide targeted visual stimuli to help users improve their accommodation ability.
*   **Variable Depth of Field Effects:** Create realistic depth of field effects not easily achievable with traditional HMD displays.