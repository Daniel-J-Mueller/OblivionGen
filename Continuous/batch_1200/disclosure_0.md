# 10031332

## Electrowetting Display with Dynamic Sub-Pixel Color Mixing

**Concept:** Enhance electrowetting displays beyond simple on/off switching of individual pixels by creating sub-pixel color mixing *within* each pixel using microfluidic channels and spectrally selective materials.

**Specifications:**

*   **Pixel Structure:** Each pixel will consist of an array of microfluidic channels arranged in a honeycomb or square grid pattern. Each channel is individually addressable via the thin-film transistor (TFT) described in the provided patent.
*   **Fluid Composition:** Each channel will contain a primary color fluid (Red, Green, Blue) dispersed in a clear, immiscible oil. The concentration of the color fluid within each channel will be dynamically adjustable.
*   **Microfluidic Control:** Each channel will have an inlet and outlet connected to a common reservoir for its respective color.  The TFT will control a micro-pump (piezoelectric or electroosmotic) within each channel, allowing for precise control of fluid flow and concentration.
*   **Spectrally Selective Reflectors/Filters:** The electrode underlying each channel will incorporate a spectrally selective reflector or filter. This will enhance the perceived color saturation by reflecting/transmitting specific wavelengths of light.  Materials to explore include dielectric multilayers or photonic crystals.  The spectral characteristics of each channel’s reflector/filter will be tuned to match its color fluid (e.g., a red reflector for the red fluid channel).
*   **TFT Integration:** The TFT’s gate terminal, as described in the patent, will be used to control the micro-pump within each channel. Precise control of the TFT switching speed is crucial for accurate color mixing.
*   **Addressing Scheme:** The display will be driven by a multiplexed addressing scheme, sequentially activating rows/columns of pixels.  The TFT switching speed and fluid response time will determine the maximum refresh rate.
*   **Optical Stack:**  A diffuser layer will be placed above the pixel array to minimize viewing angle dependence and create a more uniform display.  A polarizer layer may also be included to further enhance contrast.

**Pseudocode for Pixel Control:**

```
// For each pixel:
    // For each channel (Red, Green, Blue):
        // DesiredColorValue = TargetColor[Channel]  // Value between 0 and 1
        // CurrentFluidConcentration = ReadSensorValue(Channel)
        // DeltaConcentration = DesiredColorValue - CurrentFluidConcentration

        // If (DeltaConcentration > 0):
            // ActivateTFT(Channel)  // Initiate pump to draw in color fluid
            // PumpDuration = DeltaConcentration * CalibrationFactor
            // Wait(PumpDuration)
            // DeactivateTFT(Channel)

        // Else If (DeltaConcentration < 0):
            // ActivateTFT(Channel) //Initiate pump to push out color fluid
            // PumpDuration = Abs(DeltaConcentration) * CalibrationFactor
            // Wait(PumpDuration)
            // DeactivateTFT(Channel)
```

**Novelty:** This design moves beyond simple black and white or single-color electrowetting displays to create full-color displays with dynamically adjustable color palettes at the sub-pixel level. The integration of microfluidic control with TFT technology allows for precise control of fluid concentration and color mixing, resulting in a more vibrant and visually appealing display.  This also moves the concept away from purely optical switching, introducing a degree of analog control that significantly broadens the display's capabilities.