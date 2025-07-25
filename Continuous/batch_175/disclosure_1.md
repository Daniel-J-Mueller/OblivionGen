# 10732460

## Adaptive Quantum Dot Polarization Control

**Concept:** Enhance contrast and viewing angles in reflective liquid crystal displays by dynamically controlling the polarization state of light *within* each quantum dot, rather than relying solely on external polarizers. This moves polarization control from a macroscopic layer to a nanoscale, individual-element level.

**Specifications:**

*   **Quantum Dot Modification:** Quantum dots will be engineered with embedded, electrically-controllable anisotropy. This anisotropy will be achieved by aligning nanoscale metallic inclusions (e.g., gold nanorods) within the quantum dot matrix. The orientation of these inclusions will influence the polarization direction of emitted light.
*   **Electrode Integration:** Each quantum dot (or a small cluster) will be associated with a micro-electrode (e.g., a graphene flake) allowing for individual voltage control.
*   **Addressing Scheme:** A matrix addressing scheme (similar to an LCD) will be used to control the voltage applied to each micro-electrode. This voltage will alter the alignment of the metallic inclusions, and therefore the emitted polarization.
*   **Liquid Crystal Layer Adaptation:** The liquid crystal layer will operate primarily as a spatial light modulator (SLM), controlling the intensity of light passing through. Polarization control will be largely handled *within* the quantum dot layer. Existing dichroic dye may still be used to further refine light control.
*   **Reflector Layer Enhancement:** The reflector layer will be designed to be polarization-sensitive, enhancing reflectivity for specific polarization states. This will further boost contrast.
*   **Light Guide/LED Integration:** Existing light guide and LED configurations remain generally unchanged, focusing on efficient illumination.

**Pseudocode for Polarization Control:**

```
// For each quantum dot (QD)
function controlQDpolarization(voltage, desiredPolarizationAngle) {
  // Calculate required inclusion alignment based on desired angle
  requiredAlignment = calculateAlignment(desiredPolarizationAngle);

  // Apply voltage to micro-electrode
  applyVoltage(voltage);

  // Monitor inclusion alignment via spectroscopic feedback (optional)
  alignment = monitorAlignment();

  // Adjust voltage slightly for precise alignment if needed
  if (alignment != requiredAlignment) {
      adjustVoltage(alignment, requiredAlignment);
  }
}

// Main Display Control Loop
for each pixel {
    for each subpixel (R, G, B) {
        // Determine required polarization angle for this subpixel
        polarizationAngle = calculatePolarizationAngle(color, viewingAngle);

        // Apply voltage to control QD polarization
        controlQDpolarization(voltage, polarizationAngle);
    }
}
```

**Materials:**

*   Cadmium Selenide or Perovskite Quantum Dots
*   Gold Nanorods (aligned within QDs)
*   Graphene Micro-Electrodes
*   Transparent Conductive Oxide (TCO) layer for electrode addressing
*   Polarization-sensitive reflector coating (e.g., chiral nematic film)