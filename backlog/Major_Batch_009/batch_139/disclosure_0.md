# 9430972

## Dynamic Electrode Shaping for Enhanced Contrast & Viewing Angle

**Concept:** The core idea is to move beyond static electrode configurations in electrowetting displays and embrace dynamically reconfigurable electrodes. This isn't about *changing* the fluid configuration directly, but altering the *electrical field* influencing it to optimize viewing angles and contrast based on ambient light and user position.

**Specs:**

*   **Electrode Material:** Utilize a microfluidic elastomer (e.g., dielectric elastomer) as the primary electrode layer. This material can mechanically deform under applied voltage, allowing for precise, localized shaping of the electrode surface.
*   **Micro-Actuator Array:** Integrate a dense array of micro-actuators *beneath* the elastomer layer. These actuators, individually addressable, will control the deformation of the elastomer and therefore the shape of the electrodes. Piezoelectric or micro-electromechanical systems (MEMS) are viable options.  Actuator density: Minimum 100 actuators per square millimeter.
*   **Sensor Suite:** Incorporate a small ambient light sensor and an infrared (IR) proximity/position sensor. The light sensor will measure ambient illumination. The IR sensor will detect the user’s approximate viewing position (horizontal and vertical angle relative to the display center).
*   **Control Algorithm:** Develop a real-time control algorithm to process sensor data and calculate optimal electrode shapes. This algorithm will dynamically adjust the voltage applied to individual micro-actuators.
    *   **Algorithm Steps:**
        1.  Read ambient light level (I).
        2.  Read user viewing position (θ, φ).
        3.  Lookup pre-calculated electrode shape parameters (based on I, θ, φ) from a shape database.  The database will be generated through simulations and experimental measurements.
        4.  Apply voltages to individual micro-actuators to achieve the target electrode shape.
        5.  Repeat steps 1-4 at a refresh rate of at least 60Hz.
*   **Electrode Shape Database:**  A pre-calculated database of optimized electrode shapes for various ambient light levels and viewing angles. The database will contain parameters for controlling the deformation of the micro-actuators.  The database will be built through ray tracing simulations of light propagation through the electrowetting cell, and optimized for contrast and viewing angle.
*   **Display Stack:** Standard Electrowetting Display stack with the addition of the micro-actuator array between the substrate and the electrowetting cell.

**Pseudocode (Control Algorithm):**

```pseudocode
// Initialize sensor readings
lightLevel = readLightSensor()
viewingAngleX = readIRSensorX()
viewingAngleY = readIRSensorY()

// Lookup pre-calculated electrode shape parameters
shapeParams = lookupShapeParams(lightLevel, viewingAngleX, viewingAngleY)

// Apply voltages to micro-actuators
for each actuator in actuatorArray:
    voltage = shapeParams[actuator.id]
    applyVoltage(actuator, voltage)

// Repeat at 60Hz
```

**Innovation:**  This goes beyond simply driving the electrowetting fluid. It actively shapes the *electrical environment* around the fluid to optimize viewing characteristics.  By dynamically adjusting the electrode shape, it’s possible to improve contrast in bright light, widen viewing angles, and potentially reduce power consumption by focusing the electric field where it’s needed most. This addresses limitations related to uniform field distributions across the display surface and offers a path toward more immersive and energy-efficient electrowetting displays.