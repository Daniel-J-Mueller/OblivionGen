# 10392104

## Variable Camber Propeller with Integrated Aeroelastic Tailoring

**Concept:** A propeller blade featuring dynamically adjustable camber achieved through embedded, shape-memory alloy (SMA) actuators and a tailored aeroelastic response. The goal is to optimize blade performance across a wider range of flight conditions and reduce vibrations.

**Specifications:**

*   **Blade Material:** Carbon fiber reinforced polymer matrix composite, layered with embedded SMA wires.
*   **SMA Actuator Network:**  A dense network of Nitinol (Nickel-Titanium alloy) wires embedded within the blade’s composite structure. Wires are strategically oriented to induce local bending moments when energized.
*   **Camber Control System:** An onboard microcontroller manages the activation of SMA wires based on real-time flight parameters (airspeed, angle of attack, motor RPM, detected vibration frequencies).
*   **Sensor Suite:** Blade-mounted strain gauges, accelerometers, and pressure sensors provide feedback to the control system, allowing for precise camber adjustments and vibration mitigation.
*   **Power Supply:**  Propeller hub incorporates a miniature, high-efficiency power generator (e.g., piezoelectric or electromagnetic induction) to supply power to the SMA actuators and sensors. Alternatively, a thin, flexible power cable can run from the motor to the hub.
*   **Control Algorithm:**
    *   **Lookup Table:**  Predefined camber profiles optimized for specific flight regimes (hover, forward flight, high-speed cruise).
    *   **Feedback Loop:** Continuously adjusts camber based on sensor data to minimize drag, maximize lift, and suppress vibrations.
    *   **Vibration Mitigation:** Detects resonant frequencies and actively adjusts camber to detune the blade, reducing stress and extending its lifespan.
*   **Aeroelastic Tailoring:**  The composite layup of the blade is specifically designed to control its bending and torsional characteristics. By strategically varying the fiber orientation and thickness, the blade’s natural frequencies can be tuned to avoid resonance with engine or aerodynamic excitation.
*   **Hub Integration:** The propeller hub contains the microcontroller, power management system, and communication interface.  A wireless communication link allows for data logging and remote control.

**Pseudocode (Camber Adjustment):**

```c++
// Define flight parameters
float airspeed;
float angleOfAttack;
float motorRPM;

// Define camber adjustment ranges
float minCamber = 0.0; // Minimum camber (e.g., 0 degrees)
float maxCamber = 5.0; // Maximum camber (e.g., 5 degrees)

// Define control gains
float Kp = 0.5; // Proportional gain
float Ki = 0.1; // Integral gain

// Initialize integral term
float integral = 0.0;

// Function to adjust camber
void adjustCamber() {
  // Calculate desired camber based on flight parameters
  float desiredCamber = Kp * (airspeed - 10.0) + Ki * integral; // Example: Increase camber at lower speeds

  // Limit desired camber to valid range
  if (desiredCamber > maxCamber) {
    desiredCamber = maxCamber;
  } else if (desiredCamber < minCamber) {
    desiredCamber = minCamber;
  }

  // Calculate control signal for SMA actuators
  float controlSignal = map(desiredCamber, minCamber, maxCamber, 0, 255); // Example: PWM signal to drive SMA actuators

  // Apply control signal to SMA actuators
  setSMAActuatorSignal(controlSignal);

  // Update integral term
  integral += (desiredCamber - currentCamber) * 0.01;
}

// Main loop
void loop() {
  // Read flight parameters from sensors
  airspeed = readAirspeedSensor();
  angleOfAttack = readAngleOfAttackSensor();
  motorRPM = readMotorRPMSensor();

  // Adjust camber based on flight parameters
  adjustCamber();

  // Delay
  delay(10);
}
```

**Novelty:** This design combines active camber control with tailored aeroelasticity, allowing for optimized performance and vibration mitigation across a wider range of flight conditions. The integrated power generation system reduces reliance on external power sources, improving system autonomy.