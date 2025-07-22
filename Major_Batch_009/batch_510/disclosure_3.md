# 10067335

## Electrowetting Array with Dynamic Micro-Lens Integration

**Concept:** Integrate dynamically adjustable micro-lenses *above* each electrowetting element in the array, controlled by a separate micro-actuator network. This allows for precise control over light direction and intensity at a per-pixel level *beyond* simple color and contrast modulation, enabling advanced display effects like variable focal depth, light field displays, and holographic projections.

**Specs:**

*   **Electrowetting Element Array:** As per existing patent (10067335), forming the base pixel structure. Triangle preferred.
*   **Micro-Lens Material:** Transparent polymer (e.g., PDMS) with high refractive index.
*   **Micro-Lens Diameter:** 50-200μm (dependent on desired resolution and focal length).
*   **Micro-Lens Actuation:** Utilize Dielectrophoretic (DEP) micro-actuators. A grid of transparent electrodes *beneath* each micro-lens. Applying a voltage gradient causes localized deformation of a fluid interface within the lens, altering its focal length and direction.
*   **DEP Fluid:** A low-viscosity oil containing dielectric particles.
*   **Electrode Pattern:** Interdigitated electrodes forming a grid beneath each micro-lens. Resolution: 50-100 electrodes per mm².
*   **Control System:** Separate control matrix for DEP electrodes, synchronized with the electrowetting control. Requires high-bandwidth communication to manage individual lens adjustments.
*   **Layer Stack (Bottom to Top):**
    1.  Support Plate (existing patent)
    2.  Electrowetting Element Array
    3.  Transparent Electrode Matrix (for DEP actuation)
    4.  Microfluidic Channel (containing DEP fluid)
    5.  Micro-Lens Array (PDMS)
    6.  Protective Overcoat (optional)

**Pseudocode (Control Algorithm):**

```
For each pixel (i, j) in the array:
    // Read target image data (color, intensity)
    color = image_data[i, j].color
    intensity = image_data[i, j].intensity

    // Electrowetting Control (existing logic)
    configure_electrowetting(i, j, color, intensity)

    // Micro-Lens Control
    desired_focal_length = calculate_focal_length(i, j, depth_map) //based on depth map or desired effect
    voltage_profile = generate_voltage_profile(desired_focal_length) //calculates voltage needed on electrodes
    apply_voltage_profile(i, j, voltage_profile) //apply voltage to the pixel's DEP electrodes

    //Optional: apply dynamic 'beam steering' for holographic effects
    steering_angle = calculate_steering_angle(i, j, holographic_data)
    adjust_voltage_profile(steering_angle) //fine tune voltage to steer beam
```

**Innovation Details:** The integration of dynamic micro-lenses allows for several key advancements:

*   **3D Displays:** By varying focal length per pixel, a holographic or volumetric display can be created without the need for special glasses.
*   **Light Field Displays:** Capture and reproduce the full light field, providing a more realistic and immersive viewing experience.
*   **Adaptive Brightness/Contrast:** Dynamic adjustment of focal length can be used to enhance perceived brightness and contrast, reducing energy consumption.
*   **Beam Steering:** For advanced applications like LiDAR or optical communication.

**Potential Challenges:** Precise alignment of micro-lenses with electrowetting elements, high-bandwidth control of DEP actuators, managing heat dissipation, and achieving stable fluid interfaces.