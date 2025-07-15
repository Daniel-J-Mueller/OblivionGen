# 10059440

## Variable Stiffness Propeller Hub – Bio-Inspired Morphology

**Concept:** A propeller hub incorporating variable stiffness elements, inspired by bird wing morphology, to dynamically adjust aerodynamic performance based on flight conditions.

**Specifications:**

*   **Hub Structure:** Multi-segmented hub composed of overlapping “feather” segments arranged radially. Each segment is approximately 5-10cm long and 2-3cm wide at the base.
*   **Segment Material:** Combination of shape memory alloy (SMA) and flexible polymer composite.  SMA provides the stiffness control, while the polymer composite provides flexibility and damping.
*   **Actuation:** Each segment incorporates a micro-electrical heater (resistive element) embedded within the SMA portion. Varying the current to the heater changes the temperature and therefore the stiffness of that segment.
*   **Control System:**  An onboard microcontroller (e.g., STM32 series) receives flight data (airspeed, angle of attack, motor RPM) from sensors. This data is used to calculate the optimal stiffness profile for each segment.
*   **Sensor Suite:**
    *   Inertial Measurement Unit (IMU) – measures angular velocity and acceleration.
    *   Airspeed sensor – Pitot tube or similar.
    *   Angle of Attack (AoA) sensor – Miniature vane or pressure-based sensor.
    *   Motor RPM sensor – Hall effect sensor.
*   **Power Supply:** Integrated power supply module deriving power from the aerial vehicle's main battery. Dedicated power rails for the microcontrollers, sensors, and SMA heaters.
*   **Communication:**  CAN bus or similar for communication with the vehicle’s flight controller.
*   **Segment Arrangement:**  Alternating arrangement of “stiff” segments (high SMA content) and “flexible” segments (high polymer content) around the hub circumference. This allows for localized control of aerodynamic forces.

**Operational Algorithm (Pseudocode):**

```
// Initialize variables
airspeed = 0
AoA = 0
motorRPM = 0
stiffnessProfile[numSegments]  // Array to store stiffness values for each segment

// Main Loop
while (true) {
    // Read sensor data
    airspeed = readAirspeedSensor()
    AoA = readAoASensor()
    motorRPM = readMotorRPM()

    // Calculate desired stiffness profile based on flight conditions
    for (i = 0; i < numSegments; i++) {
        if (airspeed < threshold_low) {
            // Low Speed: Maximize lift & maneuverability
            stiffnessProfile[i] = max_stiffness
        } else if (airspeed > threshold_high) {
            // High Speed: Minimize drag & maximize efficiency
            stiffnessProfile[i] = min_stiffness
        } else {
            // Cruise Speed: Balance lift and efficiency
            stiffnessProfile[i] = calculateOptimalStiffness(AoA, motorRPM) //Implement function based on aerodynamics
        }
    }

    // Apply stiffness profile to segments
    for (i = 0; i < numSegments; i++) {
        setSegmentStiffness(i, stiffnessProfile[i]) //Control SMA heater
    }
}
```

**Materials:**

*   SMA: Nickel-Titanium alloy (Nitinol)
*   Polymer Composite: Carbon fiber reinforced polymer matrix
*   Hub Structure: High-strength aluminum alloy or carbon fiber composite

**Potential Benefits:**

*   Improved aerodynamic efficiency at varying speeds and altitudes.
*   Enhanced maneuverability and stability.
*   Reduced noise and vibration.
*   Optimized propeller performance for different flight modes (e.g., hovering, forward flight).