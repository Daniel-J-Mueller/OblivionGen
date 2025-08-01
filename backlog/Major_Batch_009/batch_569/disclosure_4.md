# 9977234

## Electrowetting Pixel Array with Dynamic Gap Control

**Concept:** A display system utilizing electrowetting principles, but incorporating microfluidic channels *within* the pixel wall structures to dynamically adjust the gap between the two support plates. This allows for pixel-level brightness and contrast control beyond what standard electrowetting displays offer, and enables grayscale manipulation without complex driving schemes.

**Specs:**

*   **Pixel Wall Material:** Photoresist-derived polymer, exhibiting sufficient structural integrity and chemical compatibility with working fluids. Microfluidic channels etched or molded *into* the wall structure during fabrication. Channel dimensions: Width = 5-10μm, Height = 2-5μm.
*   **Working Fluids:** Two immiscible fluids – an electrically conductive oil and a dielectric fluid. The conductive oil is the "pixel" fluid, manipulated by electrowetting. The dielectric fluid fills the microfluidic channels.
*   **Microfluidic Channel Network:** Each pixel wall incorporates a closed-loop microfluidic channel. A micro-pump (piezoelectric or similar) integrated into the support plate or pixel wall structure drives fluid circulation within the channel.
*   **Electrode Configuration:** Transparent electrodes (ITO or similar) deposited on both support plates, defining individual pixel areas. Additional micro-electrodes integrated *within* the pixel wall structures to control fluid flow within the microfluidic channels.
*   **Gap Adjustment Mechanism:** Increasing fluid pressure within the microfluidic channels expands the channel, physically pushing the pixel wall and support plate apart. Decreasing pressure allows the plates to move closer together.
*   **Control System:** A microcontroller-based system to individually address each pixel and control both the electrowetting voltage *and* the microfluidic pump/pressure.

**Pseudocode:**

```
// Pixel Control Loop
for each pixel in display:
    // Calculate desired brightness level (0-100%)
    desiredBrightness = // ...

    // Calculate electrowetting voltage based on desired brightness
    electrowettingVoltage = calculateElectrowettingVoltage(desiredBrightness)

    // Calculate microfluidic pressure based on desired brightness/contrast
    microfluidicPressure = calculateMicrofluidicPressure(desiredBrightness)

    // Apply electrowetting voltage to pixel electrodes
    applyElectrowettingVoltage(pixel.electrodes, electrowettingVoltage)

    // Control micro-pump to achieve desired microfluidic pressure
    controlMicroPump(pixel.microPump, microfluidicPressure)
end loop
```

**Fabrication Process:**

1.  Photoresist deposition on first support plate.
2.  Microfluidic channel patterning using advanced lithography (e.g., two-photon polymerization or nanoimprint lithography).
3.  Development of patterned photoresist to create microfluidic channel molds.
4.  Polymerization of a suitable polymer within the molds to form microfluidic channels.
5.  Deposition of pixel wall material on top of microfluidic channels.
6.  Second support plate assembly and fluid filling.
7.  Electrode deposition and interconnection.

**Potential Benefits:**

*   Enhanced contrast ratio due to precise gap control.
*   True grayscale capability without complex driving schemes.
*   Reduced power consumption through optimized gap control.
*   Potential for dynamic focusing of light for improved viewing angles.