# 9316779

## Dynamic Reflective Micro-Lenses

**Concept:** Integrate micro-lens arrays with individually controllable reflectivity to dynamically shape the light distribution across a display. This goes beyond static shaping of reflective material and moves toward *active* light management.

**Specs:**

*   **Micro-Lens Array:** Fabricate a high-density array of micro-lenses (approx. 50-100 microns diameter) on a transparent substrate. These lenses are initially highly reflective. Material: Dielectric mirrors deposited on polymer pillars or etched silicon.
*   **Electrowetting/Microfluidic Control:** Each micro-lens is coupled to a microfluidic channel filled with an electrically conductive fluid (e.g., oil with dissolved salts). Applying a voltage to the fluid alters its refractive index and, critically, its reflectivity. Higher voltage = lower reflectivity.
*   **Addressing:** Use a thin-film transistor (TFT) backplane underneath the micro-lens array to individually address each lens. Each TFT controls the voltage applied to the corresponding microfluidic channel.  Resolution: Match the micro-lens density.
*   **Light Source Integration:** Position LEDs (or laser diodes) along one or more edges of the display. The light is directed *towards* the micro-lens array.
*   **Control Algorithm:** Implement an algorithm that analyzes the desired display image and adjusts the reflectivity of each micro-lens to optimize brightness uniformity and contrast.
    *   *Image Analysis Module:*  Detects bright and dark regions in the image.
    *   *Reflectivity Mapping Module:* Generates a reflectivity map based on the image analysis, assigning reflectivity values to each micro-lens.
    *   *TFT Control Module:* Sends control signals to the TFT backplane to set the reflectivity of each micro-lens.
*   **Material Considerations:**
    *   Transparent substrate: High-transmission glass or polymer.
    *   Reflective coating: Multilayer dielectric mirror stack for high reflectivity and spectral control.
    *   Electrically conductive fluid:  Oil-based with dissolved salts for low viscosity and high conductivity.
    *   TFT Backplane: Amorphous silicon or IGZO TFTs for high resolution and low power consumption.
*   **Assembly:** Bond the micro-lens array, TFT backplane, and light sources to a rigid substrate. Encapsulate the assembly with a transparent protective layer.

**Pseudocode (Control Algorithm):**

```
function adjust_micro_lens_reflectivity(image_data):
  // image_data: 2D array of pixel intensity values (0-255)

  // Calculate average intensity
  avg_intensity = average(image_data)

  // Create reflectivity map
  reflectivity_map = 2D array of same dimensions as image_data
  for each pixel (x, y) in image_data:
    if pixel(x, y) > avg_intensity:
      reflectivity_map(x, y) = 100 // Higher reflectivity for bright areas
    else:
      reflectivity_map(x, y) = 0 // Lower reflectivity for dark areas

  // Apply reflectivity map to micro-lens array
  for each micro-lens (i, j):
    set_micro_lens_reflectivity(i, j, reflectivity_map(i, j))

end function
```

**Potential Benefits:**

*   **Superior Uniformity:** Dynamically adjust light distribution to eliminate hotspots and dark areas.
*   **Enhanced Contrast:** Increase contrast by reducing light leakage in dark regions.
*   **Improved Viewing Angle:** Shape the light to widen the viewing angle.
*   **Adaptive Brightness:** Adjust brightness based on ambient lighting conditions.
*   **Potential for Holographic Display:** Precise control over light could enable holographic display applications.