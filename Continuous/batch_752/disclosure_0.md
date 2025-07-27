# 9772488

## Electrowetting Display with Dynamic Trench Geometry

**Concept:** Implement microfluidic channels *within* the electrowetting pixel trenches, allowing for dynamic control of fluid distribution and optical properties beyond simple electrowetting effects. This allows for color mixing *within* the pixel, rather than relying on color filters or multiple subpixels.

**Specs:**

*   **Pixel Structure:** Standard electrowetting pixel stack (Support Plate -> First Conductive Layer -> First Dielectric Layer -> Second Conductive Layer -> Second Dielectric Layer -> Hydrophobic Layer) with a key modification to the trench.
*   **Trench Modification:**  The U-shaped trench (as per the patent) will *not* be simply filled with the working fluids. Instead, it will contain integrated microfluidic channels etched into the *second dielectric layer* prior to hydrophobic coating. These channels will be on the scale of 5-20um in width.  Multiple inlets/outlets will be connected to a micro-pump/valve system external to the display.
*   **Fluid System:** Two (or three) immiscible fluids:  A clear oil, and two (or one) colored oils (e.g., cyan, magenta, yellow).
*   **Channel Geometry:**  Microfluidic channels will be designed with bifurcations and mixing zones within the trench.  The geometry will be specific to each pixel, allowing for precise color mixing.  Channel designs include:
    *   **Serpentine Mixers:**  Long, winding channels to promote chaotic mixing.
    *   **Stacked Mixing Channels:** Multiple layers of microchannels stacked vertically, forcing fluids to interact.
    *   **Y-shaped bifurcations:**  Precisely controlled splitting of fluids.
*   **Electrowetting Control:** Electrowetting will still be used, *in conjunction* with the microfluidic system. Electrowetting will control the overall distribution of fluids *within* the trench and the degree to which fluids enter/exit the microchannels.  The electrowetting effect will modulate the flow rate and mixing within the microfluidic network.
*   **Micro-Pump/Valve System:**  A MEMS-based micro-pump and micro-valve array will be responsible for actively controlling fluid flow through the microchannels.  This system will be driven by a dedicated control circuit, synchronizing with the display’s refresh rate.
*   **Control Algorithm:**  A complex control algorithm will determine the appropriate combination of electrowetting voltage and micro-pump/valve settings to achieve the desired color and brightness for each pixel.  This algorithm will compensate for variations in fluid properties, temperature, and manufacturing tolerances.
*   **Materials:**
    *   Dielectric Layer:  Low-permittivity material compatible with microfabrication techniques (e.g., silicon nitride, parylene).
    *   Hydrophobic Layer:  Fluoropolymer coating to prevent fluid adhesion.
    *   Channel Material: Etched into the dielectric layer itself.

**Pseudocode (Pixel Control):**

```
// Inputs: Desired RGB value (R, G, B), Current Pixel State
// Outputs: Electrowetting Voltage (V), Pump/Valve Settings (PVS)

function ControlPixel(R, G, B, PixelState) {

  // Calculate required color mixing ratios based on R, G, B
  Ratio_ClearOil, Ratio_CyanOil, Ratio_MagentaOil = CalculateMixingRatios(R, G, B)

  //  Determine pump/valve settings to achieve mixing ratios
  PVS = CalculatePumpValveSettings(Ratio_ClearOil, Ratio_CyanOil, Ratio_MagentaOil, PixelState)

  // Calculate electrowetting voltage to optimize fluid distribution
  V = CalculateElectrowettingVoltage(Ratio_ClearOil, Ratio_CyanOil, Ratio_MagentaOil, PixelState)

  // Apply voltage and pump/valve settings
  ApplyVoltage(V)
  SetPumpValveSettings(PVS)
}
```

**Novelty:** This system moves beyond simply controlling the *shape* of the fluid interface. It enables *active* fluid manipulation within the pixel, allowing for a significantly wider color gamut, potentially exceeding the limitations of traditional color filters. The ability to dynamically mix fluids within the pixel opens possibilities for new display effects and applications.