# 10994836

## Variable Geometry Propeller Hub with Integrated Magnetic Gearing

**Concept:** A propeller hub capable of dynamically adjusting propeller pitch *and* blade count during flight via retractable/extendable blades coupled with a magnetic gear system for efficient torque transmission. This builds on the idea of clutch mechanisms controlling propeller alignment, but instead focuses on *actively* changing propeller configuration.

**Specifications:**

*   **Hub Material:** High-strength titanium alloy (Ti-6Al-4V) with internal channels for coolant circulation.
*   **Blade Material:** Carbon fiber reinforced polymer matrix composite with integrated piezoelectric actuators for fine pitch control.
*   **Blade Count:** Variable - designed for 3, 4, 5, or 6 blades. Blades slide radially inwards/outwards along keyed slots in the hub.
*   **Blade Retention:** Retaining rings and magnetic clamps secure blades in extended position.
*   **Actuation System:** Miniature linear actuators (servo-controlled) drive blade extension/retraction.
*   **Magnetic Gear System:** Halbach array based magnetic gearing positioned within the hub. Allows for torque transmission without direct mechanical connection, improving efficiency and reducing wear.
*   **Control System:** Flight computer integrated with sensors (airspeed, altitude, engine RPM) controls blade count & pitch for optimal performance.
*   **Power Source:** Dedicated high-voltage power supply for actuators & magnetic gear control.

**Pseudocode (Blade Control Logic):**

```
// Variables:
airspeed = currentAirspeed;
altitude = currentAltitude;
engineRPM = currentEngineRPM;
desiredBladeCount = 0;
bladePitch = 0;

// Define Performance Profiles:
IF (airspeed < 20 m/s AND altitude < 100m) THEN
    desiredBladeCount = 6; // Max thrust for takeoff/landing
    bladePitch = 15 degrees; // High angle of attack
ELSE IF (airspeed > 50 m/s AND altitude > 500m) THEN
    desiredBladeCount = 3; // Max speed, minimal drag
    bladePitch = 5 degrees; // Low angle of attack
ELSE IF (airspeed > 30 m/s AND altitude > 200m) THEN
    desiredBladeCount = 4; // Balanced performance
    bladePitch = 10 degrees;
ELSE
    desiredBladeCount = 5;
    bladePitch = 12 degrees;

// Actuation Sequence:
// 1. Retract/Extend Blades to achieve desired blade count (linear actuator control).
// 2. Adjust Blade Pitch via piezoelectric actuators (servo control).
// 3. Monitor Performance (airspeed, altitude, engine RPM) & adjust parameters as needed.
```

**Innovation Details:**

*   The system moves beyond simple alignment control to *active* propeller configuration.
*   Magnetic gearing eliminates traditional gearbox complexity and improves efficiency.
*   Variable blade count allows for optimal performance across a wider range of flight conditions.
*   Piezoelectric actuators enable precise pitch control for improved maneuverability.
*   This would allow for the propeller to dynamically optimize for cruise efficiency, maximum thrust, or even provide vectored thrust for enhanced maneuverability.
*   By changing the number of blades, the drag profile of the aircraft could be radically altered.