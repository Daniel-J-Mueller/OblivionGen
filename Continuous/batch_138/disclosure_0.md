# 8922869

## Electrowetting Display with Microfluidic Color Tuning

**Concept:** Integrate microfluidic channels *within* the UV-cured fluorosurfactant layer to enable dynamic color filtering *above* each pixel, enhancing display contrast and color gamut beyond traditional electrowetting.

**Specs:**

*   **Layer Stack (from substrate up):**
    1.  Substrate (glass/plastic)
    2.  Pixel Electrodes (ITO/IZO)
    3.  Inter-layer Insulation (SiO2/SiNx)
    4.  Partition Walls (Photoresist/Polymer)
    5.  **Microfluidic Fluorosurfactant Layer:** UV-cured fluorosurfactant incorporating etched microchannels.  Channel dimensions: width 5-10 µm, depth 1-2 µm, pitch 20-30 µm. This layer acts as both dielectric and fluidic substrate.
    6.  Water Repellent Layer (Fluoropolymer)
    7.  Black Oil Layer (Optional – for contrast enhancement)
*   **Microchannel Design:**  Each pixel will have multiple (3-5) parallel microchannels running across its surface.  These channels will be filled with a colored microfluid (e.g., dyes dissolved in a clear oil).
*   **Fluid Control:** Microfluidic channels connected to an external micro-pump/valve system. The system will selectively deliver different colored fluids to each pixel’s channels, dynamically adjusting color filters.
*   **Electrowetting Function:** Standard electrowetting control of water droplet/oil interface within the electrowetting cell. The microfluidic color filters operate *in addition to* the electrowetting effect.
*   **Materials:**
    *   Fluorosurfactant:  As in patent, optimized for UV curing and microchannel etching compatibility.
    *   Microfluid:  Viscosity matched to channel dimensions and actuation speed. High color saturation dyes suspended in a clear, low-volatility oil.
    *   Channel Etch: Reactive Ion Etching (RIE) with a fluorine-based plasma to selectively etch channels into the cured fluorosurfactant.
    *   Channel Sealant: Thin-film polymer sealant to prevent fluid leakage.

**Pseudocode for Dynamic Color Control:**

```
// Pixel Array[x,y] represents each pixel in the display
// ColorChannels[x,y][i] represents the i-th color channel for the pixel at [x,y]
// AvailableColors = [Red, Green, Blue, Cyan, Magenta, Yellow, White]

function setPixelColor(x, y, targetColor) {
  // Determine optimal fluid combination for target color
  fluidCombination = calculateFluidCombination(targetColor)

  // Activate/Deactivate micro-pumps to deliver fluid combination
  for (i = 0; i < fluidCombination.length; i++) {
    if (fluidCombination[i] == true) {
      activatePump(ColorChannels[x,y][i])
    } else {
      deactivatePump(ColorChannels[x,y][i])
    }
  }

  //Apply Electrowetting voltage to control droplet shape and light transmission
  applyElectrowettingVoltage(x,y, desiredBrightness)
}

function calculateFluidCombination(targetColor) {
  //Algorithm to determine the mix of fluids (red, green, blue) to achieve the target color
  //Based on color mixing principles (e.g., additive color mixing)
  //Returns an array of booleans, representing whether each color channel should be activated
  //Example: [true, false, true] would activate red and blue channels for magenta
}
```

**Potential Benefits:**

*   Expanded color gamut beyond traditional electrowetting displays.
*   Dynamic contrast control through color filtering.
*   Improved viewing angles.
*   Potential for creating novel display effects (e.g., dynamic patterns, animations).

**Challenges:**

*   Microfluidic channel fabrication within the fluorosurfactant layer.
*   Fluid control and delivery system integration.
*   Prevention of fluid leakage and contamination.
*   Long-term fluid stability and durability.