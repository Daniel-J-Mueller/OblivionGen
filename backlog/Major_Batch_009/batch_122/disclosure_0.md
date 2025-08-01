# 10235948

## Electrowetting Array with Dynamic Surface Tension Modulation

**Concept:** Integrate micro-fluidic channels *within* the support plate, directly adjacent to the hydrophobic layer, capable of delivering surfactant solutions. This allows for *localized* and *dynamic* modulation of surface tension at the fluid/support interface, independent of applied voltage.

**Specifications:**

*   **Support Plate Material:** Transparent polymer (PMMA, PDMS) or glass.
*   **Micro-channel Dimensions:** Width: 5-20µm, Depth: 1-5µm. Channel spacing: 20-50µm. Channels arranged in a grid pattern coinciding with the electrowetting pixel array.
*   **Fluid Delivery System:** Integrated micro-pumps (piezoelectric or peristaltic) controlled by the processor and memory. Reservoirs for surfactant solutions and deionized water.
*   **Surfactant Solutions:** Range of surfactants with varying HLB (Hydrophilic-Lipophilic Balance) values. Concentrations controllable via software.
*   **Electrode Configuration:** Standard ITO (Indium Tin Oxide) electrodes patterned on the substrate.  Electrode dimensions and spacing optimized for desired pixel resolution.
*   **Hydrophobic Layer:** Fluorinated polymer coating (e.g., Cytop) applied to the support plate.
*   **Control Algorithm (Pseudocode):**

```
// Variables:
//  pixel_data[x,y]: Target grayscale value for pixel (x,y)
//  voltage[x,y]: Voltage applied to pixel (x,y)
//  surfactant_flow[x,y]: Flow rate of surfactant solution to pixel (x,y)
//  base_voltage:  Default voltage for switching
//  surfactant_sensitivity: Calibration factor for surfactant effect on surface tension

function calculate_control_signals(pixel_data[x,y]):

    // Calculate voltage adjustment based on pixel value
    voltage[x,y] = base_voltage + (pixel_data[x,y] * voltage_scale)

    //Calculate surfactant flow based on deviation from target grayscale
    deviation = target_grayscale - pixel_data[x,y]

    if (deviation > threshold) {
        surfactant_flow[x,y] = deviation * surfactant_sensitivity
    } else {
        surfactant_flow[x,y] = 0
    }
    
    return voltage[x,y], surfactant_flow[x,y]
```

*   **Integration:** The micro-fluidic channels are fabricated directly into the support plate using micro-fabrication techniques (e.g., photolithography, etching). The hydrophobic layer is deposited *after* channel fabrication.  Micro-pumps and reservoirs are integrated on a separate substrate and bonded to the support plate.
*   **Calibration Procedure:** A calibration routine is required to map pixel values to optimal voltage and surfactant flow rates, accounting for variations in surface tension, fluid properties, and device characteristics.
*   **System Requirements:**
    *   High-speed data acquisition system for monitoring pixel states.
    *   Precision flow control system for regulating surfactant delivery.
    *   Real-time processing capabilities for implementing the control algorithm.



**Innovation:** This system moves beyond solely voltage-based control of electrowetting. By actively modulating surface tension via surfactant delivery, finer control over droplet shape and contact angle is achieved. This enables:

*   **Enhanced Contrast Ratio:** Improved grayscale representation and deeper blacks.
*   **Faster Switching Speeds:** Lower voltages required for droplet movement.
*   **Reduced Power Consumption:** Lower operating voltages.
*   **Dynamic Optical Effects:** Creation of complex droplet shapes and patterns for advanced displays.