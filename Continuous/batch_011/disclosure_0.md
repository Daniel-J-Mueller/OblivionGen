# 10288871

## Dynamic Light-Shaping TFT Backplane for Micro-LED Displays

**Concept:** Leverage the light-absorbing organic material layer in the TFT backplane not just for shielding, but as an *active* element for shaping and directing light emitted from micro-LEDs integrated directly above the TFT structure. This creates a dynamically adjustable brightness and color mixing system at the subpixel level *without* needing additional optical layers or complex micro-lens arrays.

**Specs:**

1.  **TFT Stack Modification:**
    *   Replace the standard passivation layer with a multi-layer dielectric stack. The bottom layer functions as standard passivation. The top layer is a thin (50-200nm) layer of electrochromic polymer doped with light-absorbing organic material (similar to the original patent’s material, but specifically selected for its electrochromic properties alongside light absorption).
    *   Integrate transparent electrodes (ITO or similar) *within* the multi-layer dielectric stack *above* the electrochromic layer. These electrodes are patterned to define subpixel-level control zones.
    *   Micro-LED placement directly above each subpixel control zone.

2.  **Electrochromic Control System:**
    *   Dedicated drive circuitry for each subpixel control zone.
    *   Apply variable voltage to the transparent electrodes. This voltage changes the refractive index and light absorption characteristics of the electrochromic layer.
    *   Voltage range: 0-10V, allowing precise control over light transmission.
    *   Control Algorithm: A lookup table correlating voltage to refractive index/absorption coefficient.  This table is calibrated during manufacturing to account for material variations.

3.  **Micro-LED Integration:**
    *   Micro-LEDs are bonded directly to the top surface of the electrochromic layer using a low-stress adhesive.
    *   LED pitch:  Variable, but targeting < 50 microns for high-resolution displays.
    *   LED color:  Red, Green, and Blue micro-LEDs patterned to create full-color subpixels.

4.  **System Operation:**
    *   The display controller sends pixel data to the TFT backplane.
    *   The TFT backplane drives the micro-LEDs.
    *   *Simultaneously*, the TFT backplane adjusts the voltage applied to the transparent electrodes above each subpixel.
    *   This adjusts the amount of light emitted from the micro-LED that reaches the viewer.
    *   Precise voltage control enables:
        *   Dynamic contrast control: Increasing brightness locally while maintaining overall power efficiency.
        *   Color mixing:  By selectively absorbing light from one or more LED colors, the perceived color of the subpixel can be fine-tuned.
        *   Viewing angle compensation: Directing light to specific areas, creating the illusion of a wider viewing angle.

**Pseudocode:**

```
// Per-Subpixel Loop
for each subpixel in display:
  // Get Pixel Data
  red = pixel_data.red
  green = pixel_data.green
  blue = pixel_data.blue

  // Drive Micro-LEDs
  drive_led(red, green, blue)

  // Calculate Voltage for Electrochromic Layer
  voltage = calculate_voltage(red, green, blue, desired_brightness, desired_color_mix)

  // Apply Voltage to Transparent Electrode
  apply_voltage(voltage)
end for
```

**Material Specifications:**

*   **Electrochromic Polymer:** Poly(3,4-ethylenedioxythiophene) polystyrene sulfonate (PEDOT:PSS) with tailored additives for improved switching speed and contrast.
*   **Light Absorbing Organic Material:**  A derivative of the material used in the original patent, chosen for its compatibility with PEDOT:PSS and its ability to efficiently absorb specific wavelengths of light.
*   **Transparent Electrode:** Indium Tin Oxide (ITO) or a transparent conductive polymer.