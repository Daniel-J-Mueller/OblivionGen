# 10054785

## Microfluidic Lens Array Fabrication

**Concept:** Leverage the anisotropic stiffness of the polymer nanocomposite resist structures – specifically the high longitudinal stiffness – to create dynamically adjustable microfluidic lenses. Instead of simply acting as spacers, these resist structures will *become* the core of a lens, with fluidic control dictating focal length and potentially astigmatism correction.

**Specs:**

*   **Material:** Polymer nanocomposite resist (as described in the base patent) with tunable refractive index. Targeting materials allowing for a refractive index change of at least 0.05 via applied electric field or thermal stimulus. Composition optimized for low hysteresis in refractive index change.
*   **Resist Structure Geometry:** Arrays of vertically aligned, cylindrical micro-pillars. Pillar diameter ranging from 1-5µm, pillar height from 5-20µm. Pillar density variable across the array for custom lensing effects. Precise control of pillar sidewall angle – ideally near vertical – to minimize light scattering.
*   **Microfluidic Integration:** Each pillar is a sealed microchannel. The core of the pillar is filled with an electro-responsive fluid (e.g., an ionic liquid or a liquid crystal) capable of changing refractive index upon application of an electric field. Channels are sealed at top and bottom with optically transparent, electrically conductive films (ITO or similar).
*   **Electrode Configuration:** Individual micro-electrodes embedded within the bottom conductive film, directly beneath each pillar. Enables precise, independent control of the refractive index within each pillar.
*   **Substrate:** Transparent substrate (glass, quartz, or suitable polymer) patterned with micro-channels for fluid delivery and electrical connections.
*   **Assembly:**  Two substrate layers bonded together, forming a sealed microfluidic chamber containing the micro-pillar lens array. Electrical connections routed to external control circuitry.

**Operation:**

1.  An electric field is applied to the micro-electrode beneath a specific pillar.
2.  The electro-responsive fluid within the pillar changes refractive index.
3.  This alters the light focusing properties of the pillar, effectively creating a lens.
4.  By controlling the electric field applied to each pillar, the focal length and shape of the entire lens array can be dynamically adjusted.

**Pseudocode (Control Algorithm):**

```
// Define lens array dimensions (num_pillars_x, num_pillars_y)
// Define target focal length (focal_length)
// Define desired astigmatism correction (astigmatism_angle)

function adjust_lens_array(focal_length, astigmatism_angle) {
  for (i = 0; i < num_pillars_x; i++) {
    for (j = 0; j < num_pillars_y; j++) {
      // Calculate voltage required for each pillar to achieve desired focal length
      voltage = calculate_voltage(i, j, focal_length);

      // Apply astigmatism correction by adjusting voltage based on angle
      voltage = apply_astigmatism_correction(voltage, i, j, astigmatism_angle);

      // Apply voltage to corresponding electrode
      set_electrode_voltage(i, j, voltage);
    }
  }
}

function calculate_voltage(x, y, focal_length) {
  // Implement algorithm to calculate required voltage based on pillar position and desired focal length
  // Consider pillar geometry, fluid properties, and material characteristics
  // Return calculated voltage
}

function apply_astigmatism_correction(voltage, x, y, astigmatism_angle) {
  // Implement algorithm to adjust voltage based on astigmatism angle and pillar position
  // Return adjusted voltage
}

function set_electrode_voltage(x, y, voltage) {
  // Apply calculated voltage to corresponding electrode
}
```

**Potential Applications:** Miniature cameras, adaptive optics, dynamic displays, and micro-spectrometers.