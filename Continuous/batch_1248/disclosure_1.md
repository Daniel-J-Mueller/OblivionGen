# 11909950

## Dynamic Target Geometry for Multi-Sensor Calibration

**Concept:** Expand the testing apparatus to utilize actively morphing targets, rather than static arrangements, to create more complex and realistic validation scenarios. This allows for dynamic occlusion, varied surface normals, and programmable feature arrangements, pushing beyond the limitations of planar or simply distanced target configurations.

**Specifications:**

*   **Target Material:** Shape memory polymer (SMP) composite with embedded micro-actuators.  The SMP provides structural integrity and the ability to hold a deformed shape. The micro-actuators (piezoelectric or micro-electromechanical systems - MEMS) provide localized deformation control.  Surface is a retroreflective material for consistent sensor readings.
*   **Actuation System:** An array of individually addressable micro-actuators embedded within each target. Control is achieved via a microcontroller (ESP32 or similar) with a wireless communication module (Bluetooth/WiFi).
*   **Target Geometry:** Targets will not be strictly planar. They will be capable of forming concave and convex surfaces, folds, twists, and dynamically adjustable angles. Initial target shapes are triangular prisms, with adjustable angles between faces.
*   **Control Software:** A software suite that allows users to:
    *   Define target sequences (deformation patterns over time).
    *   Programmatically control individual actuators.
    *   Import pre-defined sequences or generate them algorithmically.
    *   Synchronize target movements with sensor data acquisition.
    *   Real-time visualization of target deformation.
*   **Sensor Integration:** Software integration with the 3D sensor to trigger data acquisition synchronized with target movements. Triggering is done via software interface.
*   **Calibration Procedure:**
    1.  A user defines a validation sequence of target deformations. The sequence may include:
        *   Target occlusion (targets moving to block/unblock sensor view).
        *   Surface angle variation (changing the angle of a target’s surface).
        *   Target approach/recession (targets moving closer/further from the sensor).
    2.  The software controls the target actuators to execute the defined sequence.
    3.  The 3D sensor simultaneously captures range data.
    4.  The validation software analyzes the captured data to assess the sensor's performance under dynamic conditions, looking for data quality and accuracy.

**Pseudocode - Target Control:**

```
// Target Initialization
function initializeTarget(targetID, actuatorPins[]) {
  for each pin in actuatorPins {
    set pin as OUTPUT
    set pin LOW // deactivate actuator initially
  }
}

//Actuator Control
function controlActuator(actuatorPin, dutyCycle) {
  analogWrite(actuatorPin, dutyCycle) // Send PWM signal to control actuator power
}

//Shape Function
function setShape(targetID, shapeName) {
  if (shapeName == "flat") {
    //Actuator control for flat state. Set duty cycle values for each pin.
  } else if (shapeName == "concave") {
    //Actuator control for concave state.
  } else if (shapeName == "convex"){
     //Actuator control for convex state
  }
  //More shapes defined similarly
}

//Sequence Execution
function executeSequence(sequence[]) {
  for each step in sequence {
    setShape(step.targetID, step.shape)
    delay(step.duration)
  }
}
```

**Innovation Potential:**

*   **Realistic Testing:** Provides more realistic validation scenarios than static targets.
*   **Dynamic Occlusion Analysis:** Allows for testing sensor performance in dynamic occlusion scenarios.
*   **Feature Richness:**  Complex surface deformations create a variety of features for sensor to analyze.
*   **Sensor Robustness:** Tests sensor robustness to environmental changes.