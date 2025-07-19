# 9576398

## Dynamic Focal Plane AR Display

**Concept:** Augment the light shutter technology to create a dynamic focal plane for projected AR imagery, reducing eye strain and improving perceived depth. Current AR displays often present imagery at a fixed focal distance, forcing the eye to accommodate or causing visual fatigue. This system aims to dynamically adjust the focal plane of the projected image to match the perceived depth of virtual objects, making the AR experience more natural and comfortable.

**Specs:**

*   **Display:** Semi-transparent waveguide display, similar to current AR glasses, but with a higher refresh rate (at least 240Hz) for smoother focal plane adjustments.
*   **Light Shutter Array:**  A dense array of individually controllable micro-shutters *behind* the waveguide. Each shutter controls the polarization of ambient light *and* a corresponding micro-lens element.
*   **Micro-Lens Array:** A layer of dynamically adjustable micro-lenses positioned *between* the shutter array and the waveguide. Each micro-lens is paired with a corresponding shutter element.
*   **Depth Sensing:** Integrated depth sensor (e.g., Time-of-Flight or structured light) to map the user's environment and the position of virtual objects.
*   **Processing Unit:** High-speed processor to calculate the required focal plane adjustments for each virtual object and control the shutter/lens array.
*   **Polarization Control:** Each shutter element modulates the polarization of ambient light and projects a polarized component. The micro-lens shapes and directs this polarized light.
*   **Waveguide Optimization:** The waveguide material is engineered to maximize polarization-dependent transmission.

**Functionality:**

1.  **Depth Mapping:** The depth sensor generates a depth map of the environment and tracks the position of virtual objects.
2.  **Focal Plane Calculation:** The processing unit calculates the desired focal plane for each virtual object based on its distance from the user.
3.  **Shutter/Lens Control:**  For each pixel displaying a virtual object:
    *   The corresponding shutter element modulates the ambient light polarization, enhancing the visibility of the projected light.
    *   The paired micro-lens adjusts its shape to focus or defocus the light, effectively changing the perceived depth of that pixel. This is achieved by controlling the refractive index gradient within the micro-lens.
4.  **Dynamic Adjustment:** The shutter/lens array dynamically adjusts in real-time to track the movement of virtual objects and maintain a consistent focal plane. The system prioritizes focusing on areas where the user is actively looking (determined by eye-tracking).
5.  **Ambient Light Blending:** The shutter elements also modulate ambient light transmission, seamlessly blending virtual objects with the real world.

**Pseudocode (Simplified):**

```
// For each pixel (x, y) on display:
  // Get depth value (depth_map[x, y])
  depth = depth_map[x, y]

  // Calculate focal distance (focal_distance) based on depth
  focal_distance = calculate_focal_distance(depth)

  // Calculate micro-lens curvature (curvature) based on focal distance
  curvature = calculate_curvature(focal_distance)

  // Set micro-lens shape
  set_micro_lens_shape(x, y, curvature)

  // Set shutter polarization based on object color and intensity
  set_shutter_polarization(x, y, object_color, object_intensity)
```

**Potential Benefits:**

*   Reduced eye strain and fatigue
*   Improved perceived depth and realism
*   More immersive AR experiences
*   Enhanced visual comfort
*   Potentially reduced motion sickness.