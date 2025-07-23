# 10903612

## Dynamic Conformable Dock with Haptic Feedback

**Concept:** A dock utilizing shape-memory alloy (SMA) actuators to dynamically conform to device geometry *and* provide haptic feedback to the user regarding connection status and power transfer. This expands beyond simple physical conformity by adding a sensory element.

**Specifications:**

*   **Dock Body:** Constructed from a rigid polymer (e.g., polycarbonate) with integrated channels for SMA wire routing. Internal structure designed to distribute load and minimize stress on SMA elements.
*   **Actuation System:** An array of individually controlled SMA wires embedded within the dock's contact surfaces (both top and bottom sections). Each wire connected to a microcontroller and driver circuit.
*   **Sensing Layer:** A capacitive or resistive pressure sensing layer overlaid on the SMA wire network. This provides feedback to the microcontroller regarding contact pressure and device presence.
*   **Electrical Contacts:** Pogo pins as described in the provided patent, but with significantly reduced spring force.  The SMA network will provide the primary clamping force. Consider using multiple layers of pogo pins for redundancy.
*   **Microcontroller:**  ARM Cortex-M series microcontroller with sufficient PWM outputs to control individual SMA wires and ADC inputs for the pressure sensing layer.
*   **Power Delivery:** Integrated USB-C Power Delivery (PD) controller to negotiate and provide optimal power to the docked device.
*   **Haptic Feedback:**  The SMA wires will not only conform to the device but also subtly vibrate or pulse to signal connection events (docked, charging, full charge, errors).  These vibrations are controlled by the microcontroller.  Different vibration patterns signify different states.
*   **Software:**
    *   **Calibration Routine:** Automated calibration process to map the pressure sensor data to device geometry and adjust SMA wire actuation accordingly.
    *   **Device Recognition:** Algorithm to identify the docked device based on pressure profile and electrical characteristics.
    *   **Haptic Control:** Software module to generate and control haptic feedback patterns.
    *   **Error Handling:**  Mechanism to detect connection failures and provide appropriate haptic and visual alerts.

**Pseudocode (Haptic Feedback Control):**

```
FUNCTION GenerateHapticPattern(patternType)
  SWITCH patternType
    CASE "Docked":
      // Short, single pulse
      Activate SMA Wire Array (amplitude = 0.1, duration = 50ms)
    CASE "Charging":
      // Slow, rhythmic pulse
      LOOP
        Activate SMA Wire Array (amplitude = 0.2, duration = 200ms)
        Delay(500ms)
      END LOOP
    CASE "FullCharge":
      // Three short pulses
      FOR i = 1 TO 3
        Activate SMA Wire Array (amplitude = 0.3, duration = 100ms)
        Delay(200ms)
      END FOR
    CASE "Error":
      // Rapid, irregular pulses
      // (Implement random pulse generation)
    DEFAULT:
      // No haptic feedback
  END SWITCH
END FUNCTION

ON DockedEvent:
  GenerateHapticPattern("Docked")

ON ChargingEvent:
  GenerateHapticPattern("Charging")

ON FullChargeEvent:
  GenerateHapticPattern("FullCharge")

ON ErrorEvent:
  GenerateHapticPattern("Error")
```

**Novelty:**  This goes beyond simple physical docking by adding a dynamic, adaptable, and sensory experience. The haptic feedback enhances usability and provides clear communication to the user. The combination of SMA actuation and a pressure sensing layer enables a truly "smart" dock that can conform to and interact with a wide range of devices. It's a shift from passive retention to active engagement.