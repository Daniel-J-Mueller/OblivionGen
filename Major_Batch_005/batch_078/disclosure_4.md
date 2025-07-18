# D1013547

## Modular Kinetic Charging Dock - "FlowState"

**Concept:** A charging dock incorporating kinetic energy harvesting *and* aesthetic fluid dynamics. The dock isn't static; it subtly *moves* in response to environmental vibrations (footsteps, music) and the device being charged, creating a visually calming and subtly interactive experience.

**Specs:**

*   **Base Unit:** Circular base, 15cm diameter, constructed of brushed aluminum alloy. Contains the primary charging circuitry (USB-C PD, Qi wireless).  Internal weighted gimbal for stability.
*   **Kinetic Harvesters:** Six miniature linear generators arranged radially within the base.  Each generator consists of a spring-loaded mass attached to a coil.  Ambient vibrations and deliberate touches cause the mass to oscillate, generating electricity.  Output is rectified and used to supplement the charging power or power integrated LEDs.
*   **Fluid Core:** A sealed, transparent acrylic cylinder (8cm diameter, 10cm height) sits atop the base. Filled with a non-conductive, high-viscosity fluid (mineral oil or similar) and small, polished spheres (e.g., ceramic beads).
*   **Magnetic Actuation:** Six miniature, programmable electromagnets are embedded around the base of the fluid core cylinder. Controlled by a microcontroller, these magnets subtly manipulate the spheres within the fluid, creating flowing, organic patterns. The movement is synchronized with charging status (slow pulse when charging, faster/more complex patterns when fully charged).
*   **Device Cradle:** A magnetically attached, adjustable cradle designed to hold smartphones or other devices.  The cradle itself is minimal, allowing the fluid core to be prominently displayed.  Wireless charging coil embedded within.
*   **Microcontroller & Sensors:**  A small microcontroller (ESP32 or similar) manages the kinetic energy harvesting, magnetic actuation, charging circuitry, and integrates data from an accelerometer (detects vibrations) and a current sensor (monitors charging current).
*   **Power Management:** Excess kinetic energy stored in a small supercapacitor.  Used to power the LEDs and magnetic actuators when external power is unavailable.

**Pseudocode:**

```
// Main Loop
while (true) {
    // Read accelerometer data
    vibrationLevel = readAccelerometer();

    // Read current sensor data
    chargingCurrent = readCurrentSensor();

    // Kinetic Energy Harvesting
    harvestedEnergy = readSupercapacitorLevel();

    //  Magnetic Pattern Generation
    if (chargingCurrent > 0) {
        if (harvestedEnergy > threshold) {
            pattern = generateComplexPattern(vibrationLevel); // Dynamic pattern based on vibration
        } else {
            pattern = generateSimpleChargingPattern(); // Simple pulse
        }
        actuateMagnets(pattern);
    } else {
        actuateMagnets(idlePattern()); // Minimal movement
    }

    delay(50ms);
}

// Pattern Generation Functions (examples)
function generateComplexPattern(vibrationLevel) {
    // Create a pattern based on vibration level and random elements
    // More intense vibrations -> more complex/faster patterns
    // Randomness keeps the movement organic
    // Return an array defining magnet activation sequence
}

function generateSimpleChargingPattern() {
    // Simple pulsing pattern to indicate charging
    // Return an array defining magnet activation sequence
}
```

**Refinements/Extensions:**

*   Biometric integration (e.g., respond to touch).
*   Ambient lighting control (integrate with smart home systems).
*   Customizable fluid color and sphere materials.
*   Haptic feedback to user on touch.
*   Data visualization - display charging data via the fluid core using LED projections.