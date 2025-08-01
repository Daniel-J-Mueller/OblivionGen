# 9557558

## Electrowetting Array with Dynamic Masking for Pixelated Displays

**Concept:** Utilize the electrowetting principle to create a high-resolution display where each ‘pixel’ is a microfluidic chamber with individually controllable droplet movement, but enhanced with a dynamic masking layer to drastically improve contrast and viewing angle.

**Specs:**

*   **Substrate:** Glass or flexible polymer base.
*   **Electrode Layer:** ITO (Indium Tin Oxide) patterned into a high-density array of micro-electrodes. Electrode dimensions: 5-20 µm.
*   **Microfluidic Chamber Layer:** PDMS (Polydimethylsiloxane) microfluidic chambers fabricated on top of the electrode layer. Chamber dimensions: 20-50 µm diameter, 10-30 µm height. Chambers filled with two immiscible fluids: a colored oil and a clear conductive fluid (e.g., water with a dissolved electrolyte).
*   **Dynamic Masking Layer:** A layer of micro-particles suspended in a clear, electrically controllable fluid (dielectrophoretic fluid) situated *above* the microfluidic chamber layer. Particle size: 1-5 µm. Particle material: High refractive index material (e.g. TiO2).
*   **Top Electrode Layer:** Transparent top electrode layer (ITO or similar) to apply voltage across the dynamic masking layer.
*   **Control System:** Array of micro-drivers connected to both the electrowetting electrodes and the top electrode for the dynamic masking layer.

**Operation:**

1.  **Electrowetting Control:** Applying a voltage to an individual electrode causes the colored oil droplet to move within its microfluidic chamber, effectively ‘turning on’ that pixel.
2.  **Dynamic Masking:** Applying a voltage to the top electrode creates an electric field that attracts the micro-particles towards the ‘on’ pixels. This creates a localized high-refractive index layer *above* the illuminated pixels, enhancing light extraction and blocking light from adjacent pixels.  This is especially critical for black levels.
3.  **Contrast Enhancement:** By dynamically controlling the particle distribution, the system can dramatically improve contrast.  'Off' pixels remain dark due to the particle distribution blocking any ambient light or bleedthrough.
4.  **Viewing Angle Improvement:**  The refractive index gradient created by the particles helps bend light, widening the effective viewing angle.
5.  **Gray Scale:** Control of the applied voltage to both the electrowetting electrode *and* the masking layer allows fine control of light transmission, enabling grey scale display.

**Pseudocode for Control System:**

```
// Pixel data structure
struct Pixel {
  int electrowettingVoltage;
  int maskingVoltage;
  int color; // Red, Green, Blue values
}

// Function to update display
function updateDisplay(Pixel pixelArray[], int arraySize) {
  for (int i = 0; i < arraySize; i++) {
    setElectrowettingVoltage(i, pixelArray[i].electrowettingVoltage);
    setMaskingVoltage(i, pixelArray[i].maskingVoltage);
  }
}

// Helper functions (implementation details)
function setElectrowettingVoltage(int pixelIndex, int voltage) {
  // Apply voltage to corresponding electrowetting electrode
}

function setMaskingVoltage(int pixelIndex, int voltage) {
  // Apply voltage to corresponding masking electrode
}
```

**Further Research:**

*   Optimization of particle size and concentration for optimal light extraction and contrast.
*   Development of novel dielectric fluids with high permittivity and low viscosity.
*   Exploration of different particle materials and surface treatments to enhance optical properties.
*   Integration with flexible substrates for roll-to-roll manufacturing.