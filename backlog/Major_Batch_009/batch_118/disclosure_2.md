# 12202295

## Modular, Bio-Inspired Wheel System with Variable Stiffness

**Concept:** Develop a wheel system employing a modular design incorporating individually controlled, bio-inspired ‘muscles’ to dynamically adjust wheel stiffness and compliance. This goes beyond simply absorbing impacts – it *actively* shapes the wheel’s response to terrain.

**Specs:**

*   **Wheel Structure:** Wheel consists of a central hub, a rigid outer ring, and a network of interconnected ‘modules’ filling the space between.
*   **Modules:** Each module is a self-contained unit approximately 5cm x 5cm x 3cm. It contains:
    *   A small, high-torque servo motor.
    *   A shape memory alloy (SMA) actuator or a dielectric elastomer actuator (DEA). The chosen actuator will dictate the range of motion and force output.
    *   A miniature force sensor (piezoelectric or similar).
    *   A microcontroller with wireless communication (Bluetooth Low Energy).
    *   A small battery for autonomous operation (rechargeable via inductive charging).
*   **Interconnection:** Modules connect to each other and the hub/outer ring via a ball-and-socket joint allowing multi-axis movement. Connection points have integrated wiring for power and communication.
*   **Control System:** A central processor (on the robot) communicates wirelessly with each module.  The control algorithm:
    *   Uses input from the robot's sensors (cameras, lidar, IMU).
    *   Analyzes terrain features.
    *   Calculates the optimal stiffness and compliance for each module to maximize traction, stability, and ride comfort.
    *   Sends commands to the modules to adjust their actuation level.
*   **Actuation Mechanism:**
    *   Each module contains a spring mechanism with the SMA or DEA controlling the spring compression/extension.
    *   By varying the actuation level, the effective stiffness of the connection point between the hub and outer ring is altered.
*   **Terrain Adaptation Modes:**
    *   *Rough Terrain:* Modules soften, allowing the wheel to conform to obstacles and maximize contact area.
    *   *Smooth Terrain:* Modules stiffen, providing a rigid, efficient rolling surface.
    *   *Turning:* Modules on the outside of the turn stiffen, while those on the inside soften, creating a banking effect.
    *   *Obstacle Negotiation:* Individual modules around the impact point soften to absorb energy, while others remain stiff to maintain structural integrity.

**Pseudocode (Control Loop – per module):**

```
// Input: Terrain Data (from central processor), Module Position
// Output: Actuation Level (servo angle)

function calculate_actuation_level(terrain_data, module_position):
    if terrain_data[module_position] == "obstacle":
        actuation_level = soften()  // Activate servo to allow SMA/DEA to compress spring
    else if terrain_data[module_position] == "rough":
        actuation_level = moderate_soften()
    else if terrain_data[module_position] == "smooth":
        actuation_level = stiffen() // Activate servo to tension spring
    else:
        actuation_level = maintain_current_level()

    return actuation_level
```

**Materials:**

*   Hub & Outer Ring: Lightweight aluminum alloy or carbon fiber composite.
*   Module Housing: High-impact plastic (e.g., polycarbonate).
*   Interconnection Joints: Stainless steel or titanium alloy.
*   SMA/DEA:  Nickel-titanium alloy or suitable dielectric elastomer.

**Future Work:**

*   Integrate machine learning to predict terrain features and optimize actuation levels.
*   Develop a self-healing mechanism for the interconnection joints.
*   Explore the use of energy harvesting to power the modules.