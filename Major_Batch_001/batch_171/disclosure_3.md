# 10126626

## Electrically-Actuated Haptic Feedback Layer

**Concept:** Integrate a microfluidic layer *within* the electrically printable medium to create localized haptic feedback. The electrical signals driving the visible content also control actuators within this layer, creating tactile sensations corresponding to the displayed information.

**Specs:**

*   **Layer Stack (from bottom to top):**
    1.  First Flexible Structural Layer (PET, as per patent)
    2.  Moisture Barrier Coating (as per patent)
    3.  First Conductor Layer (Transparent, for electronic ink control AND microfluidic actuation)
    4.  First Electronic Ink Layer (as per patent)
    5.  Insulator Layer (thin, prevents cross-talk between ink and actuation layers)
    6.  Second Conductor Layer (Patterned for microfluidic channel control AND secondary electronic ink layer)
    7.  Second Electronic Ink Layer (Optional – for multi-layer display)
    8.  Microfluidic Layer:
        *   Material: PDMS (Polydimethylsiloxane) – biocompatible, flexible, microfabricatable.
        *   Channel Dimensions: 200μm wide, 100μm high (adjustable based on desired tactile resolution)
        *   Channel Density: 50 channels/cm² (adjustable)
        *   Fluid: Dielectric fluid (low viscosity, high dielectric constant)
    9.  Actuation Mechanism: Electrowetting-on-Dielectric (EWOD)
        *   Each microfluidic channel has integrated EWOD electrodes controlled by the second conductor layer.
        *   Applying a voltage to an electrode changes the surface tension of the dielectric fluid, causing it to move within the channel, creating a localized bulge.
    10. Second Flexible Structural Layer (Moisture Barrier, as per patent)
    11. Protective Outer Layer (Scratch Resistant, Anti-Glare, as per patent)

*   **Electrical Control:**
    *   Printer sends combined visual and haptic data.
    *   Control software maps visual elements to corresponding haptic regions.
    *   Second Conductor Layer receives both display and haptic control signals.
    *   Software adjusts voltage to EWOD electrodes to control bulge height and duration, creating various tactile sensations (bumps, ridges, vibrations).

*   **Pseudocode (Haptic Control):**

```
function generateHapticSignal(visualElement, intensity) {
  //visualElement = {x, y, width, height}
  //intensity = 0.0 - 1.0

  //Map visual element to corresponding haptic region(s)
  hapticRegions = mapVisualToHaptic(visualElement)

  for each region in hapticRegions {
    //Calculate voltage based on intensity
    voltage = intensity * maxVoltage

    //Send voltage signal to corresponding EWOD electrodes
    sendSignal(region.electrodeIDs, voltage)
  }
}

function mapVisualToHaptic(visualElement) {
  //Logic to map visual element coordinates to haptic region/electrode IDs.
  //Could use a lookup table, or a more complex algorithm.
  //Return array of haptic region objects {electrodeIDs}
}
```

*   **Potential Applications:**
    *   Interactive displays for visually impaired users.
    *   Enhanced gaming experiences.
    *   Realistic simulations (e.g., medical training).
    *   Tactile maps and diagrams.
    *   Product prototyping with realistic textures.