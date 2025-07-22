# 10656408

## Electrowetting Display with Microfluidic Color Tuning

**Concept:** Integrate microfluidic channels *within* the pixel spacer to allow for dynamic color tuning beyond simple reflectance changes in an electrowetting display. Essentially, create a miniature, pixel-level color mixing system.

**Specs:**

*   **Spacer Material:** Transparent, chemically resistant polymer (e.g., PDMS, PMMA). The spacer will function as both structural support *and* the microfluidic channel network.
*   **Channel Dimensions:** Microchannels etched/molded *within* the pixel spacer, average width 5-20 microns, depth 1-5 microns. Channel layout designed for optimal mixing within the pixel area.
*   **Fluid Composition:** Three primary color fluids (Cyan, Magenta, Yellow) stored in micro-reservoirs integrated with the display periphery. Fluids must be compatible with electrowetting operation (low viscosity, dielectric properties) and stable over time.
*   **Actuation:** Electrowetting-driven micro-pumps integrated at the fluid inlet of each pixel's microchannel network. These pumps control the flow rate of each color fluid into the pixel. 
*   **Control System:** A pixel-level control circuit that regulates the voltage applied to each micro-pump based on the desired color output. A look-up table maps voltage to color mixing ratios.
*   **Electrode Layer Modification:** The electrode layer on the organic layer is modified to include micro-reservoirs near each pixel opening for the fluids. 
*   **Spacer Geometry:** The pixel spacer is designed with multiple interconnected microchannels forming a mixing chamber within the pixel area. Fluid entry/exit points are aligned with the micro-reservoirs.
*   **Spacer Height:** 20-50 microns.  Sufficient height to accommodate fluid flow and mixing without interfering with electrowetting operation.

**Pseudocode (Color Control):**

```
// Input: Desired RGB color values (Red, Green, Blue) - 0-255
// Output: Voltage signals for Cyan, Magenta, Yellow micro-pumps

function calculatePumpVoltages(red, green, blue) {
  // Convert RGB to CMY (Cyan, Magenta, Yellow)
  cyan = 255 - red;
  magenta = 255 - green;
  yellow = 255 - blue;

  // Scale CMY values to voltage range (e.g., 0-5V)
  cyanVoltage = map(cyan, 0, 255, 0, 5);
  magentaVoltage = map(magenta, 0, 255, 0, 5);
  yellowVoltage = map(yellow, 0, 255, 0, 5);

  return [cyanVoltage, magentaVoltage, yellowVoltage];
}

// Example Usage:
desiredRed = 200;
desiredGreen = 50;
desiredBlue = 100;

pumpVoltages = calculatePumpVoltages(desiredRed, desiredGreen, desiredBlue);

// Apply voltages to respective micro-pumps
applyVoltage(cyanPump, pumpVoltages[0]);
applyVoltage(magentaPump, pumpVoltages[1]);
applyVoltage(yellowPump, pumpVoltages[2]);
```

**Refinement Notes:**

*   Fluid sealing is critical.  Microchannel walls require surface treatment to prevent leakage and maintain fluid integrity.
*   Power consumption of the micro-pumps needs to be minimized through efficient pump design and control algorithms.
*   The control system must compensate for fluid viscosity changes due to temperature fluctuations.
*   Integration with existing electrowetting display manufacturing processes will require careful consideration.
*   Alternative fluids (e.g., dichroic dyes) could be used to expand color gamut and functionality.