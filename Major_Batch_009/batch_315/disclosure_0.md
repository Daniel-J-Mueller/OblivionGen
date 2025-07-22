# 10676176

## Modular Propeller Shielding - Bio-Inspired Deployment

**Concept:** A system utilizing segmented, bio-inspired 'scales' around the propellers for dynamic impact protection and airflow management, expanding on the existing adjustable shielding concept but moving away from solid surfaces.

**Specs:**

*   **Scale Material:** Flexible polymer composite with embedded shape memory alloy (SMA) wires.  Material choice prioritizes low weight, high flexibility, and durability.  Potential materials include TPU with carbon fiber reinforcement.
*   **Scale Geometry:**  Each 'scale' is roughly teardrop-shaped, approximately 10-15cm long and 5-8cm at its widest point.  Slight curvature allows for overlapping arrangement.
*   **Mounting:** Scales are mounted to a ring structure surrounding each propeller, using a dovetail or similar secure but low-friction connection. The ring is affixed to the motor arm.  Mounting allows for independent articulation of each scale.
*   **Actuation:**  Each scale is controlled by at least one SMA wire running through its body.  Applying current to the SMA wire causes it to contract, changing the scale’s angle.
*   **Sensor Integration:** Each scale incorporates a micro-impact sensor (piezoelectric or similar) to detect collisions.  Data is fed back to the central controller.
*   **Control System:** A central controller manages the scales. Algorithms dictate scale behavior based on:
    *   **Flight Mode:** Hover/Vertical Takeoff/Landing (VTOL) – scales extend for maximum protection. Horizontal flight – scales retract for minimal drag.
    *   **Proximity Detection:**  Using onboard sensors (LiDAR, radar, cameras), the system identifies obstacles. Scales extend/rotate to deflect airflow or provide a physical barrier.
    *   **Impact Detection:** Upon impact, scales adjacent to the collision point immediately extend/rotate to absorb energy and protect the propeller.
    *   **Aerodynamic Optimization:** Algorithm learns the optimal scale configuration for different flight conditions and adjusts accordingly, minimizing drag while maximizing safety.
*   **Power:** Scales powered via thin, flexible wiring running through the mounting ring.
*   **Redundancy:** Each scale functions independently. Failure of a single scale does not compromise the entire system.

**Pseudocode (Scale Control Algorithm):**

```
FUNCTION ControlScale(scaleID, obstacleDistance, flightMode, impactDetected):
    IF flightMode == "HOVER" OR flightMode == "VTOL":
        scaleAngle = 90 degrees // Extend for maximum protection
    ELSE:
        scaleAngle = 0 degrees // Retract for minimal drag

    IF obstacleDistance < thresholdDistance:
        //Rotate scale toward obstacle
        obstacleAngle = CalculateAngleToObstacle(obstaclePosition)
        scaleAngle = obstacleAngle

    IF impactDetected:
        scaleAngle = 180 degrees //Maximize energy absorption

    ActuateScale(scaleID, scaleAngle)
END FUNCTION
```

**Innovation:** Moving beyond solid shielding to a dynamic, responsive system inspired by biological scales (e.g., pinecone scales, reptile scales). This allows for optimized aerodynamics in normal flight while providing superior impact protection and obstacle avoidance. The system’s responsiveness and adaptability enhance safety and expand operational capabilities.