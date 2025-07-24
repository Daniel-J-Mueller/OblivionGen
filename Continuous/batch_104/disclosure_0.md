# 11104294

## Variable Geometry Wheel System for AGV Shock Absorption

**Concept:** Integrate actively deformable wheel structures into the AGV design to absorb impact energy *before* it reaches the body and payload. This moves the energy absorption focus from the body itself to the point of initial contact, offering potentially superior protection and reduced stress on the internal mechanisms.

**Specs:**

*   **Wheel Structure:** Each wheel is constructed from a series of interlocking, hexagonal 'cells' composed of a high-strength, flexible polymer (e.g., TPU). These cells are partially filled with a non-Newtonian fluid (magnetorheological fluid preferred) contained within sealed compartments.
*   **Actuation:** Each cell is independently controllable via micro-actuators (piezoelectric or small linear motors). These actuators adjust the stiffness and damping characteristics of each cell in real-time.
*   **Sensor Suite:** Each wheel incorporates an array of accelerometers, gyroscopes, and force sensors to detect impending impacts and terrain variations.
*   **Control System:** A dedicated microcontroller processes sensor data and adjusts the stiffness of individual wheel cells to optimize shock absorption. This is tied to the main AGV controller, but operates with high frequency and minimal latency.
*   **Modes of Operation:**
    *   **Normal Operation:** Cells maintain a moderate stiffness for efficient rolling.
    *   **Impact Detection:** Upon detecting an impact, cells nearest the point of contact rapidly soften, absorbing kinetic energy.
    *   **Terrain Adaptation:** The system can dynamically adjust wheel stiffness to maintain stability on uneven surfaces.
*   **Power:** Integrated micro-batteries (solid-state preferred) power the actuators and sensors within each wheel. Wireless charging via the AGV chassis.
*   **Diameter:** Wheel diameter customizable, but baseline 16-20 inches.
*   **Width:** Wheel width customizable, but baseline 8-12 inches.
*   **Material:** Hexagonal cell structure – TPU (thermoplastic polyurethane). Non-Newtonian fluid - Magnetorheological fluid. Actuators - Piezoelectric or small linear motors.
*   **Communication:** Wireless communication (Bluetooth Low Energy) between wheels and central controller.

**Pseudocode (Simplified Control Loop - Single Wheel Cell):**

```
// Initialization
sensorData = readSensors()
targetStiffness = baselineStiffness

// Main Loop
while (AGV is operational) {
    sensorData = readSensors() // Accelerometer, Force Sensor
    impactDetected = checkImpact(sensorData)

    if (impactDetected) {
        impactForce = calculateImpactForce(sensorData)
        targetStiffness = calculateTargetStiffness(impactForce) // Dynamic calculation based on force
        actuateCell(targetStiffness)
    } else {
        // Maintain baseline stiffness
        actuateCell(baselineStiffness)
    }
    delay(1ms)
}
```

**Potential Benefits:**

*   Superior shock absorption compared to passive systems.
*   Reduced stress on the AGV chassis and payload.
*   Improved stability on uneven terrain.
*   Potential for self-leveling and obstacle negotiation.
*   Adaptable to a variety of AGV sizes and payloads.