# 10268035

## Electrowetting Display with Microfluidic Lens Array

**Concept:** Integrate a microfluidic lens array *within* the electrowetting display, dynamically adjusting focal length and creating a 3D display effect without requiring glasses. This builds on the fluid control demonstrated in the patent, shifting from pixel-level color control to depth and focus control.

**Specs:**

*   **Display Structure:** Standard electrowetting layer as described in the provided patent, layered *above* a secondary microfluidic layer.
*   **Microfluidic Layer:** Composed of a matrix of micro-chambers, each containing a clear, immiscible fluid (e.g., fluorinated oil). Chambers are sized and spaced to act as microlenses.
*   **Actuation:** Each microchamber is individually addressable via microelectrodes patterned *below* the chamber. Applying a voltage alters the surface tension of the fluid within the chamber, changing its shape and focal length. This is driven by similar electrowetting principles as the primary display layer.
*   **Electrode Pattern:** Microelectrode array is layered *beneath* the microfluidic layer, patterned to correspond to individual microlenses. Electrodes are insulated from the electrowetting fluid with a thin dielectric layer.
*   **Fluid Compatibility:** The fluid within the microchambers must be immiscible with both the electrowetting fluids *and* the dielectric insulation layer. The fluid should have a high refractive index to maximize lensing effect.
*   **Addressing Scheme:** Individual microlenses are addressed with a greyscale voltage to control the degree of fluid deformation, thus precisely controlling focal length.
*   **Control System:**
    *   A depth map generator processes incoming visual information.
    *   A mapping algorithm converts depth information into voltage signals for each microlens.
    *   A microcontroller drives the electrode array based on the mapped voltage signals.
*   **Materials:**
    *   Substrates: Glass or flexible plastic.
    *   Partition walls: Similar materials as in the reference patent.
    *   Electrowetting fluid: Perfluorocarbon.
    *   Microfluidic fluid: Fluorinated oil with high refractive index.
    *   Microelectrodes: Indium Tin Oxide (ITO) or similar transparent conductive material.
    *   Dielectric layer: Silicon Nitride or similar.

**Pseudocode (Control System):**

```
// Input: Depth Map (2D array of depth values)
// Output: Voltage Signals for each Microlens

function generateMicrolensVoltages(depthMap):
  // 1. Calibrate depth values to voltage range (0-5V)
  voltageMap = calibrateDepthToVoltage(depthMap)

  // 2. Apply smoothing filter to reduce artifacts
  smoothedVoltageMap = applySmoothingFilter(voltageMap)

  // 3. Map smoothed voltages to individual microlens addresses
  microlensVoltages = mapVoltageToMicrolens(smoothedVoltageMap)

  return microlensVoltages
```

**Innovation Details:**

This design doesn't simply display 2D images; it dynamically creates a 3D image by manipulating the focal point of light emanating from each pixel. The electrowetting principles from the original patent are leveraged for precise fluid control, but applied to a new problem: optical depth. This allows for interactive 3D displays without requiring specialized eyewear. The challenge is miniaturization and precise control of the microfluidic layer, but it builds directly on the core technology described in the patent.