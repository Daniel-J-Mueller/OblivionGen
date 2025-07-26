# 9494788

## Dynamic Sub-Pixel Color Mixing via Micro-Chamber Electrowetting

**Concept:** Extend the electrowetting principle to individually control micro-chambers containing *different* colored fluids, creating a sub-pixel color mixing system. This moves beyond grayscale and allows for full color display at a very fine granularity.

**Specs:**

*   **Display Architecture:** Array of micro-chambers. Each chamber is bordered by electrodes and contains a mixture of two immiscible fluids – a clear, low-viscosity oil and a colored dye (RGB primaries plus black/white for contrast control). Chamber size: 5-10 microns diameter. Chamber depth: 1-2 microns.
*   **Electrode Configuration:** Each chamber has four independently addressable electrodes – one for each side.  Electrodes are deposited on a substrate using thin film deposition techniques (sputtering, CVD). Electrode material: Indium Tin Oxide (ITO) or similar transparent conductive material.
*   **Lyophobic Layer:**  A chemically modified surface coating the interior of each chamber, rendering it lyophobic to both the clear oil and the colored dye. Material: Fluorinated silane-based coating.
*   **Fluid Properties:**
    *   Clear Oil: Low viscosity, high dielectric constant. Example: Silicone oil.
    *   Colored Dye: Highly concentrated, non-polar dye soluble in the clear oil. RGB primaries and black/white for full color control.
*   **Addressing Scheme:** Each chamber is individually addressable via a matrix addressing scheme similar to LCDs, using a thin film transistor (TFT) backplane.
*   **Control Algorithm:**
    *   Electrowetting Voltage: Apply a voltage to the electrodes surrounding a chamber to alter the surface tension and induce movement of the colored dye within the chamber.
    *   Color Mixing: By controlling the voltage applied to each electrode, the position and concentration of the colored dye can be precisely controlled, enabling a full range of colors to be displayed.
    *   Dynamic Range: Adjust voltage to control the degree of mixing - from full saturation to complete transparency.

**Pseudocode (Chamber Control):**

```
FUNCTION ControlChamber(chamberID, redVoltage, greenVoltage, blueVoltage):
    // Input: Chamber ID, Red, Green, Blue voltage levels (0-5V)
    // Output: Chamber displays the specified color

    SET RedElectrodeVoltage(chamberID, redVoltage)
    SET GreenElectrodeVoltage(chamberID, greenVoltage)
    SET BlueElectrodeVoltage(chamberID, blueVoltage)

    // Calculate combined voltage effect on dye movement
    combinedVoltage = (redVoltage + greenVoltage + blueVoltage) / 3

    // Adjust dye position based on combined voltage
    IF combinedVoltage > threshold:
        moveDyeTowardsPositiveElectrode() // e.g., towards higher voltage
    ELSE:
        moveDyeTowardsNegativeElectrode()

    // Update chamber display based on dye position and concentration
    displayChamberColor()
END FUNCTION
```

**Refinement Notes:**

*   **Microfluidic Channels:** Integrate microfluidic channels into the substrate to replenish fluids lost due to evaporation or leakage.
*   **Encapsulation:** Fully encapsulate the micro-chambers to prevent fluid leakage and contamination.
*   **Polarization Control:** Add a polarization layer to enhance contrast and viewing angle.
*   **Haptic Feedback:** Explore the possibility of using electrowetting to create micro-actuators for haptic feedback.