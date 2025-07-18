# 9423607

## Dynamic Subpixel Geometry & Polarization Control

**Concept:** Rather than fixed subpixel arrangements, implement micro-mechanical deformation of subpixel boundaries *during* display operation. Couple this with dynamically adjustable polarization films layered *above* each subpixel.

**Specifications:**

*   **Subpixel Construction:** Each subpixel will be built on a MEMS (Micro-Electro-Mechanical Systems) foundation. This foundation will consist of a grid of tiny, individually addressable actuators. These actuators can *slightly* expand or contract, altering the shape and area of each subpixel in real time.  Range of motion: +/- 5% area change.  Actuation method: Electrostatic or piezoelectric. Resolution: Actuators positioned every 50 microns.
*   **Polarization Layers:** Above each subpixel, a thin-film polarization rotator will be positioned.  This rotator can change its angle of polarization between 0° and 90° (relative to a predetermined reference).  Control: Individual addressing of each polarization film via transparent electrodes.  Material: Liquid crystal or similar electro-optic material.
*   **Fluid Control:** Electrowetting layer as described in the patent is maintained, but fluid volume *within* each subpixel is also dynamically controlled via micro-pumps integrated into the MEMS layer. This allows for variable fluid height and light scattering.  Pump Capacity: Nanoliters.
*   **Addressing Scheme:**  Each subpixel is addressed with a 5-bit control signal: 2 bits for MEMS actuation (shape/area), 2 bits for polarization rotation, and 1 bit for fluid volume adjustment.
*   **Material Stack (bottom to top):** Support plate, MEMS layer (actuators), Electrowetting layer, Transparent electrodes (polarization control), Polarization films, Top plate/protective layer.

**Operational Pseudocode:**

```
FOR EACH subpixel IN display:
    // Calculate target shape/area based on desired pixel color and contrast
    targetArea = calculateTargetArea(desiredColor, desiredContrast, subpixelPosition)
    // Calculate target polarization angle based on desired viewing angle and color accuracy
    targetPolarization = calculateTargetPolarization(viewingAngle, desiredColor)
    // Adjust MEMS actuators to achieve target area
    adjustMEMS(subpixel, targetArea)
    // Adjust polarization rotator to achieve target angle
    adjustPolarization(subpixel, targetPolarization)
    // Calculate desired fluid volume for optimal light scattering
    targetVolume = calculateTargetVolume(desiredColor, desiredContrast)
    // Adjust micro-pump to achieve target volume
    adjustFluidVolume(subpixel, targetVolume)
END FOR
```

**Innovation Summary:** This system moves beyond static subpixel arrangements to create a truly dynamic display.  By adjusting subpixel shape, polarization, and fluid volume *in real time*, it allows for:

*   **Wider Viewing Angles:** Polarization control minimizes color shift at extreme angles.
*   **Improved Contrast Ratio:** Dynamic shaping and fluid control optimize light scattering and absorption.
*   **Enhanced Color Accuracy:**  Real-time adjustments compensate for variations in viewing conditions.
*   **Potential for 3D Display:**  Precise control of polarization and shape could enable autostereoscopic 3D without the need for glasses.