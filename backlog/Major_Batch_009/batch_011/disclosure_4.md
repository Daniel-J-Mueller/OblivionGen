# 9223127

## Dynamic Fluidic Lens Array for Electrowetting Displays

**Concept:** Integrate microfluidic lensing with electrowetting displays to create dynamically adjustable focal length elements *within* each pixel, effectively creating a display capable of both image display *and* integrated optical magnification/reduction. This moves beyond flat displays to a volumetric display concept.

**Specifications:**

1.  **Pixel Structure:** Each pixel consists of a standard electrowetting cell (as described in the provided patent) *integrated* with a microfluidic channel. The channel is filled with a clear, immiscible fluid (different from the fluids within the EW cell) which forms a lens.

2.  **Lens Actuation:**  A secondary, smaller EW cell controls the shape of the lens fluid. Applying voltage to this cell alters the surface tension, changing the lens’s curvature and therefore, focal length. This is done *independently* for each pixel.

3.  **Microfluidic Channel Dimensions:** Channels are approximately 50-100 microns wide, 200-500 microns long, and 20-50 microns deep. These dimensions are optimized to allow for rapid fluidic response while maintaining optical clarity.

4.  **Electrode Configuration (Lens Control):**  The secondary EW electrodes for lens control are patterned in a concentric ring arrangement within the microfluidic channel.  Applying different voltages to these rings will create asymmetrical lens deformation for more complex optical effects (e.g., astigmatism correction).  These electrodes are insulated from the primary EW cell electrodes.

5.  **Fluid Selection:**  The lens fluid is a highly purified, non-conductive oil with a precisely controlled surface tension and refractive index.  It must be immiscible with both EW fluids and compatible with the channel materials.

6.  **Control System:** A dedicated microcontroller monitors and adjusts the voltages applied to both the primary EW cell electrodes (for image display) *and* the secondary EW electrodes (for lens control). This system utilizes a pre-calibrated lookup table to map desired focal lengths to corresponding voltage levels.

7.  **Addressing Scheme:**  Each pixel has individual control over both the image data *and* the lens focal length, requiring a high-resolution addressing scheme. A thin-film transistor (TFT) backplane provides independent control of each pixel's image and lens data.

**Pseudocode for Focal Length Adjustment:**

```
// Global Variables
float desiredFocalLength; // in mm
float calibrationConstantA; // Experimentally determined constant
float calibrationConstantB; // Experimentally determined constant

// Function: adjustFocalLength(pixelX, pixelY)
// Adjusts the voltage applied to the lens control electrodes for a specific pixel
function adjustFocalLength(pixelX, pixelY) {

  // Calculate voltage based on desired focal length and calibration constants
  float voltage = calibrationConstantA * desiredFocalLength + calibrationConstantB;

  // Clamp voltage to safe operating range (e.g., 0-5V)
  voltage = constrain(voltage, 0, 5);

  // Send voltage signal to corresponding pixel's lens control electrodes
  sendVoltageSignal(pixelX, pixelY, voltage);
}

// Main Loop
loop {
  // Get desired focal length for each pixel (based on application requirements)
  desiredFocalLength = getDesiredFocalLength();

  // Iterate through each pixel
  for (int x = 0; x < displayWidth; x++) {
    for (int y = 0; y < displayHeight; y++) {
      // Adjust focal length for the current pixel
      adjustFocalLength(x, y);
    }
  }

  // Update display image data
  updateDisplayImage();
}
```

**Potential Applications:**

*   **Variable magnification displays:** Zoom in/out without pixelation.
*   **3D displays without glasses:** Create depth perception by focusing each pixel at a different distance.
*   **Adaptive optics:** Correct for visual distortions in real-time.
*   **Microscopic imaging:** Integrated magnification for portable devices.
*   **Holographic displays:** Controlled light manipulation for creating holographic images.