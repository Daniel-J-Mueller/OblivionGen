# D1040158

## Dynamic Texture Shifting Cover

**Concept:** An electronic device cover incorporating microfluidic channels and patterned, light-reactive materials to allow for dynamic, user-customizable textural and visual patterns on the device surface. Essentially, the case *feels* different based on user input or pre-set conditions.

**Specs:**

*   **Material:** Cover composed of a rigid, transparent polymer (polycarbonate or similar) with embedded microfluidic channels. The channel dimensions are on the order of 0.1mm - 0.5mm wide, and 0.05mm - 0.2mm deep.
*   **Microfluidic Network:**  A network of these channels forms a grid-like pattern across the rear surface of the cover. Channel design should allow for both individual control of small sections *and* broader, sweeping effects.
*   **Reactive Fluid:** The channels are filled with a non-conductive, viscous fluid containing micro-particles with controllable optical and tactile properties. Possible particle types:
    *   **Electro-Rheological Fluid:** Fluid viscosity changes with applied electric field. Allows for 'hardening' or 'softening' specific areas, creating raised or recessed textures.
    *   **Photochromic Particles:** Particles change color upon exposure to light. Enables dynamic color patterns.
    *   **Micro-capsules containing Shape Memory Polymers:** Capsules rupture with heat, activating shape memory polymers to change local surface texture.
*   **Actuation System:**
    *   **Micro-heaters:** Embedded beneath the microfluidic channels, providing localized heating for shape memory polymer activation.  Resolution: 1mm spacing.
    *   **Micro-electrodes:**  Embedded alongside channels for electro-rheological fluid control.  Resolution: 0.5mm spacing.
    *   **Micro-LED Array:**  A dense array of micro-LEDs placed beneath the cover, capable of projecting light patterns onto the photochromic particles. Resolution: 0.2mm spacing.  Individual LED control.
*   **Control System:**
    *   **Bluetooth Connectivity:** Case connects to the device via Bluetooth.
    *   **Mobile App:** Dedicated app allows the user to:
        *   Select pre-programmed patterns (e.g., pulsing texture, reactive to notifications).
        *   Create custom patterns using a graphical interface.
        *   Set conditions for pattern activation (e.g., phone call, low battery).
        *   Control color and texture intensity.
    *   **Internal Processor:** Small processor integrated into the case manages the actuation system and Bluetooth communication.

**Pseudocode (Pattern Creation/Activation):**

```
// App sends desired pattern data to case processor
function sendPattern(patternData) {
    // patternData: { type: "pulse", color: "red", intensity: 75 }
    Bluetooth.send(patternData);
}

// Case processor receives pattern data
onReceive(data) {
    patternType = data.type;
    color = data.color;
    intensity = data.intensity;

    switch (patternType) {
        case "pulse":
            activatePulse(color, intensity);
            break;
        case "notification":
            activateNotification(color, intensity);
            break;
        case "custom":
            applyCustomPattern(data.patternData); // Array of channel/actuator commands
            break;
    }
}

function activatePulse(color, intensity) {
    // Sequentially activate micro-LEDs/micro-heaters/electrodes
    for (i = 0; i < channelCount; i++) {
        setLEDColor(i, color);
        setActuatorIntensity(i, intensity);
        delay(50ms);
        setLEDColor(i, "off");
        setActuatorIntensity(i, 0);
    }
}
```

**Potential Variations:**

*   **Haptic Feedback Integration:** Combine with small linear resonant actuators (LRAs) to provide tactile feedback synchronized with the visual/textural patterns.
*   **Environmental Sensing:** Integrate sensors (temperature, humidity) to create patterns responsive to the environment.
*   **Biofeedback Integration:** Connect to wearable sensors (heart rate, skin conductance) to create patterns responsive to the user's physiological state.