# 9465207

## Electrowetting Display with Dynamic Spacer Height Control

**Concept:** Introduce microfluidic channels *within* the spacers themselves, allowing for dynamic adjustment of spacer height during display operation. This enables precise control over the electrowetting cell gap, optimizing for viewing angle, contrast, and power consumption.

**Specs:**

*   **Spacer Material:** Primarily a photopolymer similar to those used in standard electrowetting displays, but with embedded microchannels. Channels are fabricated using a two-photon polymerization process *before* spacer formation, ensuring channel integrity. Channel dimensions: 50nm - 200nm diameter, 10um - 50um length. Channel density: 10-20 channels per spacer.
*   **Fluid:** A low-viscosity, electrically insulating fluid (e.g., fluorinated oil) fills the microchannels. Fluid must exhibit minimal hysteresis and be compatible with the electrowetting liquid.
*   **Actuation:** Each spacer incorporates micro-piezoelectric actuators integrated within the photopolymer, directly influencing the fluid volume within the microchannels. Actuators are controlled via a dedicated driver circuit.
*   **Spacer Geometry:** Spacers are cylindrical or elliptical, mirroring existing designs. Microchannels run lengthwise along the spacer.
*   **Grid Integration:** Spacer grid fabrication remains largely similar to the reference patent, but with an added step for actuator and channel integration.
*   **Control System:** A dedicated microcontroller manages the piezoelectric actuators based on viewing angle requirements and display content. An algorithm adjusts spacer height dynamically to optimize contrast and viewing angle.
*   **Bottom Plate Integration:** Pixel walls on the bottom plate incorporate micro-ports aligning with the spacer micro-ports. These ports are sealed to maintain fluid containment.
*   **Top Plate Integration:** The top plate (containing the electrowetting fluid and electrodes) is aligned with the spacer grid, and a sealing layer is applied to prevent fluid leakage.

**Pseudocode (Control Algorithm):**

```
// Variables
desiredViewingAngle = user input
currentSpacerHeight = sensor reading
targetSpacerHeight = calculateSpacerHeight(desiredViewingAngle)
error = targetSpacerHeight - currentSpacerHeight

// Control Loop
while (abs(error) > threshold) {
    adjustment = Proportional + Integral + Derivative(error)
    applyVoltageToPiezoActuators(adjustment)
    currentSpacerHeight = sensor reading
    error = targetSpacerHeight - currentSpacerHeight
}

// Function: calculateSpacerHeight(viewingAngle)
//    // Uses a pre-calibrated lookup table or mathematical model
//    // to determine the optimal spacer height for a given viewing angle.
//    return optimalHeight;
```

**Refinement Notes:**

*   Sensor Integration: Miniaturized capacitive or optical sensors will be embedded within the spacers to monitor actual cell gap height.
*   Fluid Containment: Robust sealing materials and microfluidic designs are critical to prevent fluid leakage over time.
*   Actuator Optimization: Precise control over actuator voltage and current will be necessary to achieve accurate spacer height adjustments.
*   Thermal Management: Heat dissipation from the actuators should be considered to prevent display malfunction.