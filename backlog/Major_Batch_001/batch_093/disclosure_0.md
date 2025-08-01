# 10071596

## Variable Friction Omni-Wheel

**Concept:** A wheel incorporating dynamically adjustable roller friction, allowing for precise control of lateral movement and rotation. Inspired by the omnidirectional aspects of the referenced patent, this design moves beyond simple sideways motion to offer controlled slip and variable friction profiles.

**Specs:**

*   **Wheel Diameter:** 20cm - 50cm (scalable)
*   **Core Material:** High-strength aluminum alloy
*   **Rim Structure:** Dual-rim construction (similar to patent), but with internal cavities.
*   **Roller Material:** High-durometer polyurethane or silicone rubber.
*   **Roller Count:** 24-48 rollers per ring (configurable)
*   **Roller Mounting:** Each roller mounted on a miniature linear actuator.
*   **Actuator Type:** Piezoelectric or micro-servo.
*   **Actuator Control:** Individual microcontroller per ring, networked via SPI/I2C.
*   **Friction Adjustment Range:** 0.1 - 1.0 (relative scale, 1.0 = maximum friction).
*   **Power Supply:** Internal rechargeable battery (LiPo) or external power input.
*   **Communication:** Wireless (Bluetooth/WiFi) for remote control and data logging.

**Detailed Description:**

The Variable Friction Omni-Wheel utilizes a dual-rim structure, similar to the referenced patent, housing two annular rings of rollers.  However, each roller is individually mounted on a miniature linear actuator. This actuator allows precise control over the roller’s extension/retraction, and therefore its contact force with the ground.

The control system operates as follows:

1.  **Input:** The system receives desired movement commands (forward/backward, left/right, rotation, or a combination). These could originate from a remote control, onboard computer, or external sensor data.

2.  **Control Algorithm:**  A control algorithm determines the necessary friction profile for each roller based on the desired movement. This algorithm will consider factors like wheel speed, ground surface, and desired trajectory.

3.  **Actuator Control:** The control algorithm sends signals to the individual linear actuators, adjusting the extension/retraction of each roller.  This alters the contact force and therefore the friction between the roller and the ground.

4.  **Movement Execution:** By selectively adjusting the friction of individual rollers, the wheel can achieve precise control over its movement. For example:
    *   **Forward/Backward:**  Increase friction on rollers at the rear/front.
    *   **Left/Right:** Increase friction on rollers on the right/left.
    *   **Rotation:**  Differentially adjust friction around the wheel perimeter.
    *   **Controlled Slip:**  Reduce friction on specific rollers to allow controlled slippage for maneuvers.

**Pseudocode (simplified control loop):**

```
// Constants
float MAX_FRICTION = 1.0;
float MIN_FRICTION = 0.1;

// Variables
float desired_speed_x;
float desired_speed_y;
float desired_rotation_rate;

// Function to calculate friction for each roller
function calculate_roller_friction(int roller_index) {
  // Simplified example - more complex logic needed for real-world application

  float friction = 0.5; // Default friction

  // Adjust friction based on desired speed
  if (desired_speed_x > 0) {
    friction += desired_speed_x * 0.1; // Increase friction on right side
  } else if (desired_speed_x < 0) {
    friction += abs(desired_speed_x) * 0.1; // Increase friction on left side
  }
  if(desired_speed_y > 0) {
    friction += desired_speed_y * 0.1
  } else if (desired_speed_y < 0) {
    friction += abs(desired_speed_y) * 0.1
  }

  // Limit friction to range
  friction = constrain(friction, MIN_FRICTION, MAX_FRICTION);

  return friction;
}

// Main Control Loop
loop {
  // Get desired movement commands
  get_movement_commands();

  // Calculate friction for each roller
  for (int i = 0; i < num_rollers; i++) {
    roller_friction[i] = calculate_roller_friction(i);
  }

  // Set actuator positions based on calculated friction
  set_actuator_positions(roller_friction);

  delay(10ms);
}
```

**Potential Applications:**

*   Robotics (precise maneuvering in tight spaces).
*   Autonomous vehicles (enhanced control and stability).
*   Medical devices (surgical robots with delicate movements).
*   Entertainment (interactive displays and immersive experiences).