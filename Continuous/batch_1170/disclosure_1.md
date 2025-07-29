# 10504458

## Electrowetting Haptic Display

**Concept:** Integrate localized microfluidic pressure variation with the electrowetting display to create a haptic feedback layer directly on the display surface. This allows for the creation of textures and shapes perceptible to the user, enhancing the display experience.

**Specifications:**

*   **Display Core:** Electrowetting display as per provided patent – two support plates with immiscible fluids controlled by electrodes.
*   **Haptic Layer:** A secondary layer integrated *between* the two support plates of the electrowetting display. This layer consists of an array of microfluidic chambers.
*   **Chamber Array:** Chambers are individually addressable via micro-electrodes. Each chamber contains a small volume of a non-conductive, incompressible fluid (e.g., silicone oil).
*   **Micro-Electrodes:** These electrodes, patterned onto one of the support plates, control the volume of each microfluidic chamber. Applying a voltage causes localized fluid displacement within the chamber.
*   **Pressure Modulation:** Volume displacement creates localized pressure variations on the surface of the display visible to the user as a texture or shape.
*   **Control System Integration:** The existing control system must be expanded to include addressing and control of the micro-electrode array.
*   **Synchronization:** The control system will synchronize the electrowetting pixel control with the haptic pressure modulation to provide a combined visual and tactile experience.

**Pseudocode (Control System):**

```
// Input: Grey level data for visual display, haptic data (texture/shape map)
// Output: Control signals for electrowetting electrodes and haptic micro-electrodes

function UpdateDisplay(greyLevelData, hapticData) {

  // Process Grey Level Data for Electrowetting Pixels
  electrowettingSignals = ProcessGreyLevel(greyLevelData);

  // Process Haptic Data for Micro-Electrodes
  hapticSignals = ProcessHapticData(hapticData);

  // Combine Signals - Simultaneously drive electrowetting and haptic electrodes
  combinedSignals = CombineSignals(electrowettingSignals, hapticSignals);

  // Send Combined Signals to Display Driver
  SendSignalsToDisplay(combinedSignals);
}

function ProcessHapticData(hapticData) {
  // hapticData is a 2D array representing pressure levels for each micro-chamber
  // Convert pressure levels to voltage signals for micro-electrodes
  microElectrodeSignals = CalculateMicroElectrodeSignals(hapticData);
  return microElectrodeSignals;
}

function CalculateMicroElectrodeSignals(hapticData) {
  //Scale and offset the haptic values based on the specific hardware.
  //Implement safeguards to prevent exceeding max voltages.
  //Return an array of voltages for each microelectrode.
}
```

**Materials:**

*   Transparent conductive materials for electrodes.
*   Hydrophobic coatings for support plates.
*   Immiscible fluids for electrowetting and haptic layers.
*   Microfluidic chamber materials (e.g., PDMS, polymers).
*   Micro-electrodes fabricated using MEMS techniques.

**Potential Applications:**

*   Enhanced gaming experience with tactile feedback.
*   Accessibility features for visually impaired users (tactile maps, Braille displays).
*   Realistic texture rendering for medical imaging.
*   Interactive educational displays.