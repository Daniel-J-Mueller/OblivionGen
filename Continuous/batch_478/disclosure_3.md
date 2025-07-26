# 9971151

**Adaptive Phosphor Layer Control for Dynamic Color Gamut**

**Concept:** The patent describes a fixed phosphor layer setup. This design explores the possibility of *dynamically* controlling the excitation of individual phosphor layers via micro-LED arrays positioned *behind* the transparent display sheet, instead of relying solely on projected light. This allows for precise color mixing *at the source*, significantly expanding the achievable color gamut and reducing light loss.

**Specifications:**

1.  **Micro-LED Array:**
    *   Resolution: Minimum 10,000 LEDs per square inch. Higher resolution desirable for finer color control.
    *   LED Types: Red, Green, and Blue Micro-LEDs.  Each LED individually addressable.
    *   Placement: Positioned directly behind the transparent display sheet, aligned with the phosphor layer pixels. Physical gap of 50-100 microns to prevent light interference.
    *   Control System: Dedicated microcontroller with PWM (Pulse Width Modulation) control for each LED. Communication via SPI or I2C.

2.  **Phosphor Layer Modification:**
    *   Reduced Phosphor Density: Phosphor layer density reduced by 50% compared to the original patent design. This reduces light scattering and maximizes the effect of the micro-LED excitation.
    *   Phosphor Material: Same as original patent (Blue, Green, Red phosphors with specified emission wavelengths).
    *   Layer Structure: Phosphor layers remain stacked, with the blue layer positioned between green and red.

3.  **Light Blocking Layer:**
    *   Blue Light Blocking Layer: Remains attached to the red phosphor layer as described in the patent.
    *   UV Blocking Layer: A secondary UV blocking layer positioned *between* the micro-LED array and the phosphor layers to prevent UV emission from affecting color accuracy and lifespan.

4.  **Control Algorithm (Pseudocode):**

```
function adjust_color(target_color: RGB) {
  // target_color: desired RGB color value (0-255)

  // Calculate required intensity for each LED based on target color and phosphor emission characteristics
  red_led_intensity = calculate_intensity(target_color.red, red_phosphor_emission_profile)
  green_led_intensity = calculate_intensity(target_color.green, green_phosphor_emission_profile)
  blue_led_intensity = calculate_intensity(target_color.blue, blue_phosphor_emission_profile)

  // Apply calculated intensities to micro-LED array
  set_led_intensity(red_led_intensity)
  set_led_intensity(green_led_intensity)
  set_led_intensity(blue_led_intensity)
}

function calculate_intensity(target_color_value, phosphor_emission_profile) {
  // This function calculates the optimal intensity for a given LED based on the target color value
  // and the emission characteristics of the corresponding phosphor layer.
  // The emission profile should include data on peak wavelength, bandwidth, and efficiency.
  // The function should consider factors such as phosphor excitation curve and light absorption.

  // Placeholder logic - replace with actual calculations.
  return target_color_value * phosphor_efficiency
}
```

5.  **Materials:**
    *   Transparent Display Sheet: High-transparency glass or acrylic.
    *   Micro-LED Array: GaN-based micro-LEDs with high efficiency and long lifespan.
    *   Encapsulation:  Transparent epoxy resin for protecting the micro-LED array and phosphor layers.

6.  **Power Requirements:** 5V DC, 2A (estimated).

7.  **Dimensions:** Scalable. Minimum unit size: 1 inch x 1 inch.



This system allows for a wider color gamut, higher contrast, and improved energy efficiency compared to solely relying on projected light excitation. The dynamic control of each phosphor layer enables real-time color correction and adaptation to different viewing conditions.