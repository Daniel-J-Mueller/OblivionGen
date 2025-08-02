# 10127868

## Dynamic Subpixel Shape Modulation

**Concept:** Adapt the physical shape of subpixels *during* operation to optimize light emission/reflection and color mixing, going beyond simple fluid control. This allows for increased contrast, wider color gamuts, and improved viewing angles.

**Specs:**

*   **Subpixel Construction:** Each subpixel is built upon a micro-electromechanical system (MEMS) foundation, specifically utilizing a flexible, transparent polymer membrane stretched over an array of individually addressable micro-actuators (piezoelectric or electrostatic).
*   **Shape Modulation Range:** The membrane can deform into a variety of shapes - convex, concave, and even slightly twisted – under actuator control. The maximum deformation will be limited to prevent membrane failure, with a deformation range of +/- 15% in both height and width.
*   **Actuator Array Density:** Minimum 100 actuators per mm^2 to provide sufficient control resolution for smooth shape transitions.
*   **Fluid Integration:** The electrowetting fluid (as described in the provided patent) remains integral, but its volume is reduced to minimize distortion during shape changes. The fluid fills the space *between* the deformable membrane and a static, transparent backplate.
*   **Control System:** A dedicated control unit receives pixel data and translates it into actuator commands. This system will include:
    *   A lookup table mapping pixel color/intensity to optimal subpixel shape profiles.
    *   Real-time feedback from micro-sensors embedded in the MEMS structure to compensate for membrane imperfections or actuator drift.
*   **Materials:**
    *   MEMS Membrane: Flexible, transparent polymer with high tensile strength (e.g., Polyimide or a specialized silicone).
    *   Actuators: Piezoelectric or Electrostatic micro-actuators.
    *   Backplate: Highly transparent glass or polymer.
    *   Electrowetting Fluid: Standard electrowetting fluid optimized for minimal distortion.

**Pseudocode - Shape Modulation Algorithm:**

```
function modulateSubpixelShape(pixelData, subpixelIndex):
    // pixelData contains RGB values (0-255)
    // subpixelIndex identifies the specific subpixel being controlled

    // 1. Determine Target Shape Profile
    shapeProfile = lookupTable[pixelData.R, pixelData.G, pixelData.B, subpixelIndex]

    // 2. Calculate Actuator Voltages
    // The shapeProfile contains a set of target heights for each actuator in the array.
    // This step calculates the voltage needed to achieve that height.
    for each actuator in actuatorArray:
        targetHeight = shapeProfile[actuator.position]
        voltage = calculateVoltage(targetHeight, actuator.calibrationData)
        setActuatorVoltage(actuator, voltage)

    // 3. Feedback and Correction (Optional)
    // Use sensor data to measure actual subpixel height and adjust voltage accordingly.

    return
```

**Operation:**

The system operates by dynamically adjusting the shape of each subpixel to enhance light manipulation.

*   **Contrast Enhancement:** Concave shapes can focus light, increasing brightness and contrast. Convex shapes diffuse light, reducing glare and improving viewing angles.
*   **Color Mixing:** By varying the shape of adjacent subpixels, the effective area and light transmission of each color can be precisely controlled, resulting in a wider color gamut and more accurate color reproduction.
*   **Viewing Angle Improvement:** By shaping subpixels to direct light toward the viewer, the effective viewing angle can be significantly increased.