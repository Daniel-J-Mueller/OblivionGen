# 10086933

## Variable Geometry Propeller Hub with Magnetic Damping

**Concept:** Enhance the redundancy and efficiency of multi-motor propeller drives by incorporating a variable geometry hub coupled with magnetic damping. This allows for dynamic adjustment of propeller blade pitch and provides an additional layer of control and stabilization, particularly in the event of motor failure.

**Specs:**

*   **Hub Material:** High-strength titanium alloy with integrated magnetic particle suspension channels.
*   **Blade Attachment:** Each propeller blade is attached to the hub via a ball-joint mechanism allowing for limited angular displacement.
*   **Actuation:** Each blade’s pitch is controlled by an array of micro-linear actuators (piezoelectric or shape-memory alloy) embedded within the hub.
*   **Magnetic Damping System:** The hub contains a network of microfluidic channels filled with a ferrofluid.  Electromagnetic coils surrounding the channels modulate the viscosity of the ferrofluid, providing adjustable damping to control hub vibrations and blade oscillations.  This is crucial for stability, especially when operating with a failed motor.
*   **Control System:** A dedicated microcontroller monitors motor performance (torque, RPM) and sensor data (blade pitch, hub vibration, air pressure).  It adjusts blade pitch and damping in real-time to optimize performance and maintain stability.
*   **Redundancy Implementation:** In the event of motor failure, the control system immediately adjusts the pitch of the remaining operational propeller blades and increases magnetic damping to compensate for the loss of thrust and maintain directional control. The failed motor’s corresponding blade(s) are feathered (rotated to minimum drag) using the linear actuators.
*   **Power Requirements:** Separate power supply for the linear actuators and magnetic damping system, independent of the primary motor power.
*   **Sensor Suite:**
    *   High-resolution encoders for precise blade pitch measurement.
    *   Accelerometers and gyroscopes to detect hub vibrations and angular velocities.
    *   Pressure sensors to monitor airflow around the propeller.
    *   Current sensors to monitor motor performance.

**Pseudocode (Control Algorithm - Simplified):**

```
// Variables:
motor1_torque, motor2_torque // Torque from each motor
blade1_pitch, blade2_pitch, blade3_pitch, blade4_pitch // Blade pitch angles
hub_vibration // Measured hub vibration level
target_thrust // Desired total thrust

// Main Loop:

// Calculate desired thrust distribution
desired_motor1_thrust = target_thrust * 0.5
desired_motor2_thrust = target_thrust * 0.5

// Calculate required motor torques based on desired thrust
motor1_torque = calculate_torque(desired_motor1_thrust, current_RPM)
motor2_torque = calculate_torque(desired_motor2_thrust, current_RPM)

// Adjust blade pitch to achieve target torque (PID control)
blade1_pitch = PID_control(desired_motor1_torque, current_motor1_torque)
blade2_pitch = PID_control(desired_motor2_torque, current_motor2_torque)

// Monitor motor performance
IF (motor1_torque < threshold OR motor2_torque < threshold) THEN
    // Motor failure detected
    IF (motor1_failed == FALSE AND motor1_torque < threshold) THEN
        motor1_failed = TRUE
        // Feather blades connected to motor 1
        blade1_pitch = -90 degrees
        blade2_pitch = -90 degrees

        // Increase magnetic damping
        damping_level = max_damping
        adjust_damping(damping_level)

        // Redistribute thrust to motor 2
        target_thrust = max_thrust // Max out thrust on working motor
    ENDIF

    IF (motor2_failed == FALSE AND motor2_torque < threshold) THEN
        motor2_failed = TRUE
        // Feather blades connected to motor 2
        blade3_pitch = -90 degrees
        blade4_pitch = -90 degrees

        // Increase magnetic damping
        damping_level = max_damping
        adjust_damping(damping_level)

        // Redistribute thrust to motor 1
        target_thrust = max_thrust // Max out thrust on working motor
    ENDIF

ENDIF

// Adjust magnetic damping based on hub vibration
damping_level = calculate_damping(hub_vibration)
adjust_damping(damping_level)
```

**Innovation:**  This system moves beyond simple redundancy (one motor failing while another takes over). By actively controlling blade pitch *and* damping, it optimizes performance even in degraded conditions. The magnetic damping adds a layer of stability that existing systems lack.  The variable geometry also allows for adjustments based on flight conditions (e.g., reducing drag at high speeds, increasing lift at low speeds).