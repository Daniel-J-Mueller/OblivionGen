# 9835848

## Dynamic Micro-Lens Array with Electrowetting Control

**Concept:** Integrate a micro-lens array *within* the electrowetting display structure, using the same electrowetting principles to *actively shape* the lenses, creating a dynamic focusing effect alongside the display function. This moves beyond simple pixel switching to provide rudimentary optical zoom and depth-of-field control at the pixel level.

**Specs:**

*   **Layer Stack:**
    1.  First Support Plate (Base) – Contains First Electrode & Wall Confinement Structures (as in provided patent).
    2.  Hydrophobic Layer – Covering the First Support Plate – defines the First Surface.
    3.  Micro-Lens Array – Array of micro-lenses formed *on* the hydrophobic layer. Each lens is a sealed cavity containing a clear fluid (e.g., fluorocarbon oil). Lens material is a flexible polymer (e.g., PDMS).
    4.  Electrowetting Control Layer – Thin film electrodes patterned *under* each micro-lens, separated from the lens fluid by a dielectric layer.  These electrodes are the Third Electrode mentioned in the provided patent.
    5.  Second Support Plate – Contains Second Electrode.
    6.  First Fluid & Second Fluid – As described in the provided patent, filling the space between the plates.

*   **Micro-Lens Dimensions:**
    *   Diameter: 50 - 200 μm (scalable)
    *   Focal Length: 100 - 500 μm (adjustable via voltage)
    *   Pitch: 100 - 250 μm (dependent on desired resolution)

*   **Electrowetting Control:**
    *   Applying a voltage to the electrode *under* a lens alters the surface tension of the fluid within the lens.  This changes the curvature of the lens's free surface, altering its focal length.
    *   The voltage applied to each lens is independently controlled, allowing for dynamic focusing and aberration correction.
    *   Voltages can be ramped to achieve smooth focusing transitions.

*   **Fluid Selection:**
    *   First Fluid:  A colored or reflective fluid for display purposes (as per the provided patent).
    *   Second Fluid:  An immiscible fluid to isolate the micro-lenses and the first fluid.
    *   Micro-Lens Fluid: A clear, low-viscosity fluorocarbon oil with a high dielectric constant.

*   **Addressing Scheme:**
    *   Matrix addressing similar to existing LCD or OLED displays.
    *   Row and column drivers control the voltage applied to each lens electrode.

**Pseudocode (Lens Control):**

```
// Function to set the focal length of a single lens
function setFocalLength(row, col, voltage) {
  // Check input validity
  if (row < 0 || row >= numRows || col < 0 || col >= numCols) {
    return;
  }

  // Apply voltage to the corresponding electrode
  setElectrodeVoltage(row, col, voltage);

  // Calculate new focal length based on voltage
  // (Empirical calibration required - map voltage to focal length)
  focalLength = calibrateFocalLength(voltage);

  // Optional: Implement feedback control for precise focusing
  // (Measure image sharpness and adjust voltage accordingly)
}

// Function to calibrate focal length based on voltage
function calibrateFocalLength(voltage) {
  // This function would store a calibration table (voltage -> focal length)
  // and return the corresponding focal length for the given voltage.
  // Example:
  if (voltage < 1V) {
    return infinity; // Lens is flat
  } else if (voltage < 2V) {
    return 500um;
  } else if (voltage < 3V) {
    return 250um;
  } else {
    return 100um;
  }
}
```

**Potential Applications:**

*   Dynamic zoom functionality in compact displays.
*   Improved depth-of-field control for enhanced 3D viewing.
*   Adaptive optics for image stabilization and aberration correction.
*   Miniature endoscopes with dynamic focusing capabilities.