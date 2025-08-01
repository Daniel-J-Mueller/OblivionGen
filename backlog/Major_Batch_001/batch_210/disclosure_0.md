# 10156714

## Electrowetting Pixel with Integrated Micro-Lens Array & Dynamic Color Mixing

**Concept:** Enhance contrast and color gamut in electrowetting displays by integrating a micro-lens array *within* each pixel and implementing a dynamic color mixing strategy utilizing multiple electrolyte fluids.

**Specs:**

*   **Pixel Structure:** Standard electrowetting pixel structure as a base – first & second support plates, pixel walls defining a volume, transistor, pixel electrode, bottom electrode, hydrophobic layer, fluid accumulation region.
*   **Micro-Lens Array:** Integrated *above* the pixel electrode, within the pixel volume. Array consists of hexagonal micro-lenses, fabricated from a transparent polymer (e.g., PDMS) via micro-molding or lithography. Lens pitch: 20-50μm. Numerical Aperture (NA): 0.3-0.6.  Each pixel contains a single micro-lens, positioned to collimate light emitted from the fluid accumulation region.
*   **Multi-Fluid System:** Replace single electrolyte fluid with three distinct fluids: Cyan, Magenta, and Yellow. Each fluid is contained within a micro-reservoir *adjacent* to the fluid accumulation region, connected via micro-channels controlled by individual micro-valves.
*   **Micro-Valve Control:** Each pixel contains three micro-valves, controlled by dedicated thin-film transistors (TFTs). TFTs regulate fluid flow from reservoirs into the fluid accumulation region. Valves are normally closed.
*   **Fluid Accumulation Region Geometry:**  The recess forming the fluid accumulation region isn’t a simple flat cavity, but a specifically shaped ‘prism’ (e.g., triangular or trapezoidal). This is critical for optimizing light scattering and mixing of the color fluids.
*   **Bottom Electrode Modification:** The bottom electrode now features patterned apertures *aligned with* the fluid reservoirs. This allows for independent electrical control of each color fluid’s surface tension, affecting fluid flow and color mixing.
*   **Control Algorithm:** A real-time control algorithm dynamically adjusts:
    *   Valve opening/closing to control the proportion of each color fluid in the accumulation region.
    *   Voltage applied to the bottom electrode apertures to modulate surface tension.
    *   Transistor drive voltage to control pixel switching.
*   **Materials:**
    *   Support Plates: Glass or flexible polymer.
    *   Pixel Walls: PDMS or SU-8.
    *   Micro-Lenses: PDMS.
    *   Electrolyte Fluids: Cyan, Magenta, Yellow, optimized for low viscosity and high color purity.
    *   Bottom Electrode: ITO or similar transparent conductor.

**Pseudocode (Control Algorithm):**

```
// Input: Target RGB value (R, G, B)
// Output: Valve control signals (V_C, V_M, V_Y), Bottom Electrode Voltages (E_C, E_M, E_Y), Pixel Transistor Drive Voltage (V_Pixel)

function controlElectrowettingPixel(R, G, B):

    // Calculate desired fluid proportions
    C = map(R, 0, 255, 0, 1)  // Cyan proportion
    M = map(G, 0, 255, 0, 1)  // Magenta proportion
    Y = map(B, 0, 255, 0, 1)  // Yellow proportion

    //Normalize proportions to sum to 1.0
    total = C + M + Y
    C = C / total
    M = M / total
    Y = Y / total

    //Set valve control signals (0-1, representing valve opening)
    V_C = C
    V_M = M
    V_Y = Y

    //Calculate bottom electrode voltages (adjust surface tension)
    //Based on fluid proportions and desired color accuracy
    E_C = C * VoltageScale
    E_M = M * VoltageScale
    E_Y = Y * VoltageScale

    // Set Pixel Transistor Drive Voltage (control pixel switching)
    V_Pixel =  CalculatePixelVoltage(R,G,B)

    return V_C, V_M, V_Y, E_C, E_Y, V_Pixel
```

**Rationale:** This design aims to overcome limitations of traditional electrowetting displays by enhancing contrast and color gamut. The micro-lens array focuses light from the fluid accumulation region, increasing brightness and clarity. The multi-fluid system allows for dynamic color mixing, enabling a wider range of colors. The precise control over fluid flow and surface tension allows for fine-tuning of color accuracy and contrast.