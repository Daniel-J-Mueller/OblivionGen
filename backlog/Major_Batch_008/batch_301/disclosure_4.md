# 10345575

## Multi-Layered Electrowetting Display with Dynamic Refractive Index Control

**Concept:** Expand beyond simple electrowetting pixel control to create a display capable of dynamically altering its refractive index *within* the pixel structure, enabling advanced light manipulation for improved contrast, viewing angles, and even holographic-like effects.

**Specs:**

*   **Layered Pixel Structure:** Each pixel comprises at least three distinct layers:
    *   **Electrowetting Layer (as per patent):** Responsible for initial light modulation through electrowetting principles.
    *   **Microfluidic Refractive Index Control Layer:** A layer containing multiple microfluidic channels filled with fluids of varying refractive indices. These channels are individually addressable via micro-electrodes.
    *   **Diffractive/Reflective Layer:** A thin film containing micro-gratings or reflectors. The pattern of these elements can be dynamically adjusted using micro-electrodes to influence light diffraction/reflection based on the refractive index state of the layer below.

*   **Addressing Scheme:** Individual micro-electrodes control both the electrowetting effect *and* the flow/state of fluids within the refractive index control layer, and the micro-gratings of the diffractive layer. This requires high-resolution electrode patterns.
*   **Fluid Selection:** Fluids within the refractive index control layer should be immiscible with the electrolyte solution and exhibit a significant difference in refractive index. The fluids must be stable over time and compatible with the microfluidic system.
*   **Control Algorithm:** An algorithm determines the optimal combination of electrowetting voltage, refractive index fluid distribution, and micro-grating pattern to achieve the desired display effect. This algorithm accounts for viewing angle and ambient lighting conditions.
*   **Microfluidic Channel Dimensions:** Channels are on the scale of 1-10 microns to facilitate rapid fluid flow and precise control. Channel arrangement is optimized for uniform refractive index distribution across the pixel.
*   **Materials:** All layers are constructed using materials compatible with thin-film manufacturing processes. Electrodes are made of transparent conductive oxides (TCOs) or other suitable materials.

**Pseudocode (Pixel Control):**

```
function controlPixel(pixel_x, pixel_y, desired_color, viewing_angle, ambient_light):

  // 1. Calculate required Electrowetting voltage based on desired_color
  electrowetting_voltage = calculateElectrowettingVoltage(desired_color)

  // 2. Determine optimal refractive index distribution
  refractive_index_map = calculateRefractiveIndexMap(desired_color, viewing_angle, ambient_light)

  // 3. Apply voltages to microfluidic channels to achieve refractive_index_map
  for each channel in refractive_index_map:
    applyVoltage(channel, refractive_index_map[channel])

  // 4. Calculate and apply micro-grating pattern based on viewing angle and refractive_index_map
  micro_grating_pattern = calculateMicroGratingPattern(viewing_angle, refractive_index_map)
  applyMicroGratingPattern(micro_grating_pattern)

  // 5. Apply electrowetting voltage to pixel
  applyElectrowettingVoltage(pixel_x, pixel_y, electrowetting_voltage)
end function
```

**Potential Benefits:**

*   Enhanced contrast and brightness
*   Wider viewing angles
*   Potential for 3D/holographic display capabilities
*   Improved energy efficiency (by manipulating light directly)