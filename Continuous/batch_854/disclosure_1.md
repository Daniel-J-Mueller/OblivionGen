# 9069161

## Microfluidic Lens Arrays for Dynamic Focal Control

**Concept:** Integrate actively controlled microfluidic lenses *within* the second support plate of the electrowetting display. These lenses dynamically adjust focal length based on applied voltage, creating a 3D display effect or enabling advanced light field control *without* mechanical components. This moves beyond simple 2D displays, offering parallax and depth cues.

**Specifications:**

*   **Lens Material:**  Transparent polymer (PDMS or similar) with high optical clarity and low viscosity.
*   **Lens Geometry:**  Micro-domes or cylindrical lenses with diameters ranging from 50-200μm.  Lens array density: 100-500 lenses per mm².
*   **Actuation Mechanism:** Each micro-lens is a sealed chamber filled with a second immiscible fluid (different from the primary second fluid used in the electrowetting cells) and contains an embedded micro-electrode. Applying a voltage to the electrode alters the surface tension of the fluid *within* the lens, changing its shape and focal length.
*   **Electrode Material:** ITO or conductive polymer.  Electrodes must be precisely patterned to avoid short circuits and ensure independent lens control.
*   **Integration with Support Plate:** The micro-lens array is fabricated directly onto the inner surface of the second support plate using microfabrication techniques (photolithography, etching, soft lithography).  A protective layer (e.g., thin film of SiO2) is deposited over the electrodes and fluid chambers to prevent leakage and corrosion.
*   **Fluid Compatibility:** The fluids used in the micro-lenses must be chemically compatible with the fluids used in the electrowetting cells to prevent cross-contamination.
*   **Control System:** A dedicated driver circuit controls the voltage applied to each micro-lens electrode, allowing for precise control of the focal length. Control algorithms could implement various effects, such as dynamic focus, depth mapping, or light field rendering.
*   **Power Requirements:**  Micro-lens actuation requires low voltage (1-5V) and low current. Power consumption should be minimized to extend battery life.
*   **Array Architecture:**  Addressable grid arrangement allowing for individual lens control.  Consider hierarchical addressing schemes to reduce wiring complexity.

**Pseudocode for Dynamic Focal Control:**

```
// Define display resolution and lens array dimensions
int displayWidth = 1920;
int displayHeight = 1080;
int lensArrayWidth = displayWidth / 10; // Example: 192 lenses wide
int lensArrayHeight = displayHeight / 10; // Example: 108 lenses high

// Function to calculate focal length based on depth map
float calculateFocalLength(float depthValue, float baseFocalLength) {
  // Apply scaling factor to adjust focal length based on depth
  float focalScale = 1.0 + (depthValue * 0.001); // Adjust scaling as needed
  return baseFocalLength * focalScale;
}

// Main Loop
for (int x = 0; x < displayWidth; x++) {
  for (int y = 0; y < displayHeight; y++) {
    // Access depth map value for current pixel
    float depthValue = getDepthValue(x, y);

    // Calculate corresponding lens index
    int lensX = x / 10;  // Map pixel to lens
    int lensY = y / 10;

    // Calculate desired focal length
    float desiredFocalLength = calculateFocalLength(depthValue, baseFocalLength);

    // Set voltage to corresponding lens
    setLensVoltage(lensX, lensY, desiredFocalLength);
  }
}
```

**Novelty:**  Existing electrowetting displays offer 2D image control. Integrating dynamically controllable microfluidic lenses adds a 3D dimension, enabling light field displays, parallax effects, and interactive 3D interfaces without the need for glasses or head-mounted displays.  The microfluidic lens approach avoids mechanical complexity, offering a compact and energy-efficient solution.