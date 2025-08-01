# 9305513

## Electrowetting Display - Dynamic Fluid Mixing for Color Generation

**Concept:** Expand beyond simple electrowetting-driven fluid alignment to achieve color mixing *within* each display element, eliminating the need for color filters or separate RGB subpixels.

**Specs:**

*   **Fluid System:** Each display element contains *three* immiscible fluids: a clear dielectric fluid (base), a primary color fluid (cyan, magenta, yellow), and a conductive fluid.
*   **Electrode Configuration:** Each pixel has *three* independently controllable electrodes:
    *   Electrode 1: Controls movement of the conductive fluid, establishing an electrical connection.
    *   Electrode 2: Controls movement of the primary color fluid (e.g., cyan).
    *   Electrode 3: Controls movement of the primary color fluid (e.g., magenta/yellow).
*   **Electrode Material:** Transparent conductive oxides (TCO) patterned with microfluidic channels.
*   **Microfluidic Design:**  Each pixel is a microfluidic chamber with precisely engineered channels to guide fluid movement. Channel dimensions and geometry are critical for mixing control.
*   **Control Algorithm:** A predictive control algorithm determines the voltage applied to each electrode to achieve the desired color and intensity. This considers fluid viscosity, dielectric constants, and channel geometry.
*   **Addressing Scheme:** Matrix addressing remains, but each pixel requires three control signals instead of one.

**Pseudocode for Color Mixing Control:**

```
// Input: Desired RGB color values (R, G, B)
// Output: Voltage values for Electrode 1, Electrode 2, Electrode 3

function calculate_voltages(R, G, B):

    // 1. Determine Cyan, Magenta, Yellow (CMY) color values from RGB
    C = 1 - R
    M = 1 - G
    Y = 1 - B

    // 2. Map CMY values to voltage ranges (0-Vmax)
    V_cyan = map(C, 0, 1, 0, Vmax) // Vmax is the maximum voltage
    V_magenta = map(M, 0, 1, 0, Vmax)
    V_yellow = map(Y, 0, 1, 0, Vmax)

    // 3. Calculate conductive fluid voltage (determines overall pixel brightness)
    //    Brightness scales with the *minimum* of the CMY voltages.
    V_conductive = min(V_cyan, V_magenta, V_yellow)

    // 4. Return the voltage values
    return (V_conductive, V_cyan, V_magenta, V_yellow)

end function
```

**Innovation Details:**

This expands beyond simple binary (on/off) control of fluid alignment to *dynamic mixing* of color fluids within each pixel. By precisely controlling the voltages applied to the electrodes, we can achieve a full spectrum of colors without relying on color filters. The conductive fluid acts as an electrical bridge, enabling the application of voltages to the color fluids. This allows for fine-grained control over color mixing. The predictive control algorithm ensures accurate color reproduction, considering the complex fluid dynamics within each pixel. This significantly reduces manufacturing complexity and power consumption.