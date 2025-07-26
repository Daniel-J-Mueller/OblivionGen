# 10071596

## Omni-Directional Wheel with Magnetic Damping & Variable Rigidity

**Concept:** A wheel leveraging the omnidirectional roller principles but incorporating adjustable magnetic damping and rigidity control for enhanced stability and maneuverability, particularly in uneven terrain or high-speed applications.

**Specifications:**

**1. Wheel Core Structure:**

*   **Rims:** Two concentric rims (inner & outer) constructed from a lightweight, high-strength composite material (carbon fiber reinforced polymer).
*   **Hub:** Central hub housing a brushless DC motor for primary wheel propulsion. Hub integrated with an IMU (Inertial Measurement Unit) for real-time orientation and movement data.
*   **Spokes:** Radial spokes connecting the hub to both rims, incorporating wiring for power and data transmission. Spokes will have embedded strain sensors.

**2. Roller System:**

*   **Roller Arrangement:** Multiple (e.g., 20-30) rollers arranged in a ring between the inner and outer rims. Rollers are not rigidly fixed.
*   **Roller Material:** Rollers constructed from a non-magnetic, high-durometer polymer with low rolling resistance.
*   **Axle Design:** Each roller mounted on a short, independent axle allowing rotation about a vertical axis. Axles supported by miniature linear bearings for smooth operation.
*   **Axle Angle:** Axles angled at 45° relative to the wheel's rotational plane (as mentioned in patent claim 12), promoting lateral movement.
*   **Magnetic Damping:** Each roller axle surrounded by a small electromagnet. Electromagnet strength controlled independently for each roller. This provides variable damping to control lateral movement and stability.

**3. Variable Rigidity Control:**

*   **Electrorheological Fluid:** Small reservoirs of electrorheological fluid within each roller’s linear bearing.
*   **Voltage Control:** Applying a voltage to the fluid changes its viscosity, thus altering the rigidity of the roller’s axle. Higher voltage = higher rigidity.
*   **Independent Control:** Each roller’s electrorheological fluid controlled independently to adapt to terrain and speed.

**4. Control System:**

*   **Microcontroller:** Dedicated microcontroller managing all wheel functions (motor control, IMU data processing, magnetic damping, rigidity control).
*   **Sensor Fusion:** IMU data combined with strain sensor data from the spokes to provide a complete picture of wheel load and orientation.
*   **Control Algorithms:** Advanced control algorithms adjusting magnetic damping and rigidity in real-time to optimize performance and stability.
    *   **Terrain Adaptation:** Wheel detects rough terrain through IMU and adjusts rigidity to absorb shocks and maintain traction.
    *   **Speed Control:** Wheel increases damping at high speeds to reduce lateral sway and improve stability.
    *   **Maneuvering:** Wheel independently controls damping on each side to assist with turning and steering.

**5. Power System:**

*   **Battery Pack:** High-capacity battery pack housed within the hub.
*   **Wireless Charging:** Optional wireless charging capability.

**Pseudocode - Terrain Adaptation Algorithm:**

```
LOOP:
    READ IMU data (acceleration, angular velocity)
    CALCULATE terrain roughness index (TRI) based on IMU data
    IF TRI > threshold_roughness:
        FOR each roller:
            INCREASE electrorheological fluid voltage
            DECREASE magnetic damping strength
    ELSE:
        FOR each roller:
            DECREASE electrorheological fluid voltage
            INCREASE magnetic damping strength
END LOOP
```

**Potential Applications:**

*   Robotics (mobile robots, exploration robots)
*   Autonomous vehicles
*   All-terrain vehicles
*   Assistive devices (wheelchairs, mobility scooters)
*   Industrial machinery (material handling robots)