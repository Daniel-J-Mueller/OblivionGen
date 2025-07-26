# 10203495

## Pixel-Selective Backlight with Micro-Lens Array

**Concept:** Integrate a micro-lens array *behind* the TFT structure and reflective layer, coupled with individually addressable micro-LEDs positioned to shine *through* the vias. This creates a pixel-level backlight, enhancing contrast and enabling dynamic brightness control *independent* of the electrowetting effect.

**Specs:**

*   **Micro-LED Array:**
    *   Density: Matched 1:1 with pixel density of the display.
    *   Material: GaN or similar high-efficiency LED material.
    *   Size: Each micro-LED approximately 5-10 μm in diameter.
    *   Addressing: Individual LEDs controlled via a secondary TFT structure layer *below* the primary TFT layer. This layer utilizes transparent conductors (ITO, AZO) and a routing network.
*   **Via Modification:**
    *   Vias widened (10-20 μm diameter) to accommodate light transmission from the micro-LEDs.
    *   Via walls coated with a highly reflective material (e.g., silver or aluminum) to maximize light output.
*   **Micro-Lens Array:**
    *   Material: Polymer (e.g., PMMA) with high transparency.
    *   Lens Shape:  Optimized for focusing light through the vias and onto the hydrophobic layer.  Consider aspheric lenses for reduced aberrations.
    *   Array Configuration:  Lenses aligned with each via.  Spacing adjustable during manufacturing for optimal performance.
*   **Barrier Layer Enhancement:** The barrier layer between the reflective layer and organic layer must incorporate light-scattering nanoparticles. This will diffuse the light coming from the micro LEDs, smoothing any 'hot spots'.
*   **Organic Layer Composition**: A two-layer organic composition. The layer in contact with the hydrophobic layer is a highly transparent dielectric. The lower organic layer has a secondary light-scattering capability to further diffuse the light.
* **Power/Data**: A dedicated layer underneath the microLED array connects each LED to a dedicated power/data line.

**Pseudocode (Control Algorithm):**

```
For Each Pixel (x, y):
    // Electrowetting Control
    Apply Electrowetting Voltage (x, y) based on desired color/grayscale.

    // Backlight Control (Independent of Electrowetting)
    Brightness_Target = CalculateTargetBrightness(x, y)  // Based on image content/ambient light
    MicroLED_Intensity(x, y) = Map(Brightness_Target, 0, 1, 0, MaxLEDIntensity)
    SetMicroLED(x, y, MicroLED_Intensity(x, y))
End For
```

**Operation:**

1.  The electrowetting layer controls light *reflection* based on voltage applied.
2.  The micro-LEDs provide a dynamically controlled *backlight* for each pixel.
3.  The combined effect creates a display with high contrast, wide viewing angles, and potentially, significantly reduced power consumption (by only illuminating pixels that need to be bright).

**Potential Benefits:**

*   Enhanced contrast ratio
*   Increased brightness
*   Reduced power consumption
*   Improved viewing angles
*   Dynamic brightness control per pixel
*   Potential for true black levels.