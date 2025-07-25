# 11614577

## Dynamic Polarity Light Guide

**Concept:** A light guide utilizing micro-electromechanical systems (MEMS) to dynamically adjust the direction and diffusion of light emitted from LEDs, enabling localized dimming, color mixing, and even basic image projection *within* the light guide itself. This goes beyond static features and allows for adaptive displays and backlights.

**Specs:**

*   **Light Guide Material:** Transparent polymer with embedded MEMS actuators.  Polymer must exhibit low birefringence.
*   **LED Arrangement:** Dense array of RGB LEDs positioned along one edge of the light guide. LEDs addressable individually.
*   **MEMS Actuator Type:**  Micro-mirrors or adjustable refractive index materials (e.g., liquid crystals) patterned on the surface of the light guide.  Mirror dimensions: 5-50 microns.  Actuator pitch: 50-200 microns.
*   **Actuator Control:** Each actuator controlled by a dedicated driver circuit integrated with the light guide. Addressability: individual actuator control.
*   **Control Algorithm:**
    *   **Basic Dimming:** Adjust actuator angle to redirect light away from the emission surface.
    *   **Color Mixing:** Control actuator angles for individual RGB LEDs to mix colors before emission.
    *   **Localized Brightness Control:** Create localized bright spots or patterns by selectively activating and angling actuators.
    *   **Dynamic Masking:** Utilize actuator arrays to create temporary ‘masks’ within the light guide, blocking light in certain areas.
*   **Layered Structure:**
    1.  LED Array
    2.  Incident Layer (light collection)
    3.  MEMS Actuator Layer (control surface)
    4.  Diffusion Layer (frosted/textured surface for uniform emission)
    5.  Emission Surface
*   **Addressing Scheme:** Row-column matrix addressing for MEMS actuators.  Consider time-multiplexed addressing to reduce wiring complexity.
*   **Power Supply:** Separate power rails for LEDs and MEMS actuators.
*   **Communication Interface:** SPI or I2C for control data.

**Pseudocode (Localized Brightness Control):**

```
function set_brightness(x, y, level):
    // x, y coordinates within light guide
    // level: brightness level (0-100)

    // Calculate actuator indices corresponding to (x, y)
    actuator_indices = calculate_actuator_indices(x, y)

    // For each actuator:
    for each actuator_index in actuator_indices:
        // Calculate target angle based on brightness level
        target_angle = map(level, 0, 100, -45, 45)  // Example angle range

        // Send angle command to actuator driver
        set_actuator_angle(actuator_index, target_angle)

function map(value, in_min, in_max, out_min, out_max):
    // Linear mapping function
    return (value - in_min) * (out_max - out_min) / (in_max - in_min) + out_min
```

**Potential Applications:**

*   Adaptive backlights for LCD/OLED displays.
*   Dynamic signage and information displays.
*   Interactive surfaces and lighting effects.
*   Embedded displays integrated into other products.
*   Holographic elements and light field displays (with sufficient actuator density).