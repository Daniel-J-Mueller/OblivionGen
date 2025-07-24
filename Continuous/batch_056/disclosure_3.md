# D999212

## Haptic-Kinesthetic Keypad with Variable Resistance & Shape-Changing Keys

**Concept:** A keypad where each key isn't just pressed, but *felt* differently, offering variable resistance and even subtle shape changes during actuation. This goes beyond simple tactile feedback (vibration) and aims for a fully kinesthetic experience, enhancing user interaction and potentially allowing for encoded commands based on *how* a key is pressed, not just *which* key.

**Specs:**

*   **Key Material:** Each key is constructed from a matrix of micro-actuators embedded within a flexible polymer shell (e.g., silicone with embedded conductive traces).
*   **Actuator Type:**  Electroactive Polymers (EAPs) or Shape Memory Alloys (SMAs) are preferred, chosen for their ability to change shape and/or stiffness when electrically stimulated.
*   **Resistance Control:** Each key incorporates a microfluidic damping system.  Tiny channels within the key contain a magneto-rheological (MR) fluid. Applying a magnetic field (via miniature electromagnets within the key) alters the fluid's viscosity, changing the resistance felt during keypress.  Resistance is programmable per-key.
*   **Shape Change:** Upon initial press, EAPs/SMAs subtly deform the key's surface – a slight "dimpling" effect. The degree of deformation is programmable.  As the press continues, the deformation can increase to provide a clear indication of full actuation.
*   **Sensor Suite:**  Each key integrates:
    *   Force sensor: Measures the pressure applied.
    *   Velocity sensor: Detects the speed of the keypress.
    *   Strain gauge: Measures the degree of key deformation.
*   **Control System:**  A microcontroller (ARM Cortex-M series) manages the sensors, actuators, and MR fluid control for each key.  Communication with a host device is via USB-C or Bluetooth.
*   **Power:**  Powered via USB-C or small rechargeable battery.
*   **Key Dimensions:** Variable, but target a similar footprint to standard keyboard keys. Height can be slightly increased to accommodate the internal components.

**Pseudocode (Keypress Handling):**

```
function keypressHandler(keyID, force, velocity):
  // Read key configuration (resistance profile, shape change profile)

  // Adjust MR fluid viscosity based on desired resistance
  setMRFluidViscosity(keyID, resistanceProfile[force, velocity])

  // Activate EAPs/SMAs to create shape change
  activateShapeChange(keyID, shapeChangeProfile[force, velocity])

  // If force exceeds threshold AND shape change complete:
  if (force > actuationThreshold AND shapeChangeComplete):
    // Register keypress event
    registerKeyPress(keyID)
    // Provide haptic feedback (optional, if desired)
  end if
end function
```

**Potential Applications:**

*   Enhanced gaming experience (different key feels for different in-game actions).
*   Accessibility for visually impaired users (distinct key feels for different functions).
*   Secure input (encoded commands based on *how* a key is pressed, adding a layer of security).
*   Musical interfaces (variable resistance can simulate different instrument actions).