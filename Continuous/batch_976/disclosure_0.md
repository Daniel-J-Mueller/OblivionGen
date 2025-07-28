# 9213180

## Dynamic Pixel Geometry with Micro-Lens Arrays

**Concept:** Integrate micro-lens arrays *within* the electrowetting display, dynamically altering pixel shape and light direction to enhance perceived resolution and viewing angle. This goes beyond simple pixel control; it actively reshapes the perceived light emission.

**Specifications:**

*   **Substrate Modification:** The first base substrate will incorporate a layer of dynamically controllable micro-lenses. These lenses won't be static; their focal length and shape will be individually addressable.
*   **Lens Material:** Dielectric elastomer material capable of significant deformation under applied voltage. A thin film of transparent conductive material will be patterned to create individual lens electrodes.
*   **Electrode Integration:** The first electrodes (controlling electrowetting) and the micro-lens electrodes will be physically separated by a thin insulating layer. Routing will be critical.
*   **Lens Array Pattern:** High-density array (e.g., 500-1000 lenses per inch) patterned directly onto the first base substrate, aligned with the pixel grid. Lens density may vary spatially for optimized viewing.
*   **Control System:** Dedicated driver circuitry for the micro-lens array. This will require significant processing power to calculate optimal lens configurations.
*   **Voltage Mapping:** A lookup table will map desired image characteristics (brightness, contrast, viewing angle) to specific voltage configurations for both the electrowetting layer and the micro-lens array.
*   **Pixel Addressing:** Each pixel will be associated with a corresponding micro-lens. (Or groups of pixels may share lenses for efficiency, at the cost of precision).
*   **Dynamic Reshaping:** Utilize the micro-lenses to *reshape* the emitted light from each pixel. This could include:
    *   **Focusing:** Increasing perceived pixel density for sharper images.
    *   **Beam Steering:** Widening the viewing angle without sacrificing brightness.
    *   **Light Shaping:** Creating custom light patterns for unique visual effects.
*   **Pseudocode (Control Loop):**

```
FOR each pixel (x, y) IN image:
    desired_brightness = image[x, y].brightness
    desired_viewing_angle = user_preference.viewing_angle

    lens_voltage = calculate_lens_voltage(desired_brightness, desired_viewing_angle, x, y)
    ew_voltage = calculate_ew_voltage(desired_brightness, x, y)

    apply_voltage(lens_electrode[x, y], lens_voltage)
    apply_voltage(ew_electrode[x, y], ew_voltage)
END FOR
```

*   **Materials:**
    *   First and Second Base Substrates: Glass or flexible polymer.
    *   Electrodes: Indium Tin Oxide (ITO) or other transparent conductive material.
    *   Dielectric Elastomer: Silicone or acrylic-based polymer.
    *   Electrowetting Fluid: Optimized for low voltage operation and high contrast.
*   **Potential benefits:**
    *   Increased perceived resolution without increasing pixel density.
    *   Wider viewing angles.
    *   Novel visual effects.
    *   Potential for 3D display capabilities.