# 10365471

## Dynamic Sub-Pixel Diffusion Layer Control

**Concept:** Implement microfluidic channels *within* the diffusion layer itself, allowing for dynamic control over the concentration and distribution of diffusing particles. This enables real-time adjustment of color mixing at the sub-pixel level, enhancing color gamut, contrast, and viewing angle.

**Specs:**

*   **Layer Stack:**
    *   Top Support Plate
    *   Microfluidic Channel Layer (polymer, etched with channels ~1-5um width)
    *   Diffusion Layer (gel-based matrix containing diffusing particles)
    *   Color Filter Layer
    *   Electrowetting Pixel/Fluid Layer
    *   Bottom Support Plate

*   **Microfluidic Channels:**
    *   Pattern:  Array of interconnected microchannels running beneath each sub-pixel. Channels are designed to allow precise introduction and removal of fluid containing diffusing particles.
    *   Control: Each sub-pixel's microchannel network is individually addressable via integrated micro-pumps/valves (piezoelectric preferred).
    *   Fluid: Diffusing particles suspended in a clear, index-matched fluid.  Particle concentration is dynamically controlled by modulating fluid flow.

*   **Diffusion Layer Composition:**
    *   Matrix: Hydrogel with high optical clarity and low birefringence.  Must be compatible with microfluidic channels.
    *   Diffusing Particles:  Quantum dots or organic dyes exhibiting high quantum yield and photostability. Particle size optimized for fast diffusion but minimized to reduce light scattering.
    *   Concentration Gradient: Initial uniform distribution of diffusing particles. Dynamic control via microfluidic channels creates localized concentration gradients.

*   **Control Algorithm:**
    *   Input: Video signal/image data
    *   Processing:
        *   Color Lookup Table (LUT): Maps input RGB values to desired diffusion particle concentrations for each sub-pixel.
        *   Concentration Mapping: Translates LUT values into microfluidic control signals.
        *   Feedback Loop: Incorporates sensor data (colorimetry) to calibrate and maintain accurate color reproduction.
    *   Output: Microfluidic control signals for individual sub-pixel channels.

**Pseudocode:**

```
// Color Mixing Control Algorithm

function updateSubpixel(subpixel_rgb, subpixel_channels) {
  // 1. Convert RGB to target diffusion particle concentration (LUT lookup)
  target_concentration = lookupLUT(subpixel_rgb);

  // 2. Calculate microfluidic control signal based on difference between current and target concentration
  delta_concentration = target_concentration - getCurrentConcentration(subpixel_channels);

  // 3. Activate/Deactivate micro-pumps/valves to adjust fluid flow
  if (delta_concentration > 0) {
    activatePump(subpixel_channels, delta_concentration * pumpRate);
  } else if (delta_concentration < 0) {
    activateValve(subpixel_channels, abs(delta_concentration) * valveRate);
  }
}

function mainLoop() {
  for each subpixel in display {
    updateSubpixel(subpixel.rgb, subpixel.channels);
  }
}
```

**Potential Benefits:**

*   Enhanced Color Gamut: Fine-tuned color mixing enables reproduction of a wider range of colors.
*   Improved Contrast: Dynamic control over diffusion allows for localized contrast enhancement.
*   Wider Viewing Angle:  Optimized diffusion minimizes color shift at off-axis viewing angles.
*   Adaptive Display: System can adapt to ambient lighting conditions and user preferences.