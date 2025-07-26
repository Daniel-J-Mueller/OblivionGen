# 9182589

## Dynamic Pixel Depth Control via Microfluidic Channels

**Concept:** Enhance electrowetting display contrast and viewing angles by dynamically adjusting the depth of the electrowetting liquid within each pixel using integrated microfluidic channels. This allows for optimization of light scattering and absorption based on ambient lighting conditions and viewing angle.

**Specifications:**

*   **Substrate Modification:** Transparent substrate incorporates a network of microfluidic channels etched *beneath* the transparent spacers. Channels run along the length and width of each pixel, forming a grid-like structure. Channel dimensions: width – 5-10 µm, depth – 1-2 µm. Materials: PDMS or similar biocompatible polymer.
*   **Reservoir Integration:** A micro-reservoir system is integrated at the periphery of the display, connected to the microfluidic channel network. This reservoir acts as a source/sink for the electrowetting liquid. Reservoir volume: scaled to the display size and pixel density.
*   **Actuation Mechanism:** Piezoelectric micro-pumps or micro-valves control the flow of electrowetting liquid between the reservoir and individual pixels. Each pixel will have an associated micro-pump/valve.  Pump/Valve control: PWM signal for precise fluid volume adjustment.
*   **Liquid Composition:** Electrowetting liquid is a mixture of oil, water, and a surfactant. Additive: nano-particles (e.g. TiO2) for enhanced light scattering control.
*   **Control System:** A dedicated microcontroller manages the micro-pumps/valves. Input: Ambient light sensor data & Viewing angle sensor data. Algorithm:  Dynamic adjustment of liquid depth within each pixel to optimize contrast and minimize glare.
*   **Pixel Structure:** Electrowetting liquid is contained *above* the transparent spacers, filling the space between the support plate and the substrate. The microfluidic channels run *under* the spacers, allowing controlled volume changes.
*   **Channel Sealing:** A hermetic seal is required between the microfluidic channel network and the electrowetting liquid chamber. Material: biocompatible adhesive or a thin film barrier layer.

**Pseudocode (Control Algorithm):**

```
// Variables:
ambientLightIntensity;
viewingAngleHorizontal;
viewingAngleVertical;
pixelDepth[x, y]; // Current liquid depth for each pixel

// Function: AdjustPixelDepth(x, y)
function AdjustPixelDepth(x, y) {
  // Calculate desired liquid depth based on ambient light and viewing angle
  desiredDepth = BaseDepth + (AmbientLightInfluence * (ambientLightIntensity - Threshold)) + (ViewingAngleInfluence * (viewingAngleHorizontal + viewingAngleVertical));

  // Limit desired depth to valid range
  desiredDepth = constrain(desiredDepth, MinDepth, MaxDepth);

  // Calculate volume change required
  volumeChange = (desiredDepth - pixelDepth[x, y]) * PixelArea;

  // Activate micro-pump/valve to move liquid volume
  ActivatePump(x, y, volumeChange);

  // Update pixel depth value
  pixelDepth[x, y] = desiredDepth;
}

// Main Loop:
loop {
  ReadAmbientLight();
  ReadViewingAngle();

  for each pixel (x, y) {
    AdjustPixelDepth(x, y);
  }
}
```

**Potential Benefits:**

*   Improved Contrast Ratio: Dynamic depth control allows for optimization of light scattering and absorption.
*   Wider Viewing Angles: Reduction of glare and enhancement of visibility from off-axis angles.
*   Adaptive Brightness:  Liquid depth can be adjusted to compensate for varying ambient light conditions.
*   Enhanced Color Saturation: Optimization of light interaction with color filter material.