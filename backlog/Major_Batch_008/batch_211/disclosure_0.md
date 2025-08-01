# 9152185

## Haptic Back-Surface Projection

**Concept:** Combine the back-surface touch sensing with localized thermal/vibration actuators to *project* haptic feedback onto the user’s hand, creating the illusion of physical buttons, textures, or even shapes.

**Specs:**

*   **Sensor Array:** High-resolution capacitive touch sensor covering a substantial portion of the device's back surface. Minimum resolution: 50 PPI.
*   **Actuator Array:** An array of miniature Peltier elements (for thermal feedback) and linear resonant actuators (LRAs) or eccentric rotating mass (ERM) motors. Actuator density should match or exceed the touch sensor density. Each actuator should be individually addressable.
*   **Thermal Range:** Peltier elements capable of generating a temperature range of 10°C – 40°C, with precise temperature control (±0.5°C).
*   **Vibration Frequency/Amplitude:** LRAs/ERMs capable of generating frequencies from 50Hz - 250Hz and variable amplitude.
*   **Control System:** A dedicated microcontroller or FPGA responsible for:
    *   Receiving touch input coordinates from the touch sensor.
    *   Calculating the appropriate thermal and vibration patterns based on the touch location and virtual UI element.
    *   Driving the Peltier elements and LRAs/ERMs accordingly.
*   **Software API:** An API allowing applications to define virtual UI elements with associated thermal and vibration profiles. This should include parameters for:
    *   Shape and size of the virtual element.
    *   Thermal temperature and gradient.
    *   Vibration frequency and amplitude.
    *   Haptic “texture” (e.g., ridged, bumpy, smooth).
*   **Engagement Gesture:** Utilize a double-tap or long-press on the back surface to “lock” the haptic feedback for a sustained interaction, preventing accidental activation.

**Pseudocode (Interaction Flow):**

```
// Application requests haptic button at coordinates (x, y)
HapticSystem.createButton(x, y, 5mm radius, 25C temperature, 100Hz vibration)

// User touches back of device
touchX = TouchSensor.getX()
touchY = TouchSensor.getY()

// Calculate distance from touch to button center
distance = sqrt(pow(touchX - buttonX, 2) + pow(touchY - buttonY, 2))

if distance < buttonRadius:
    // Activate haptic feedback
    PeltierArray.setTemperature(buttonX, buttonY, 25C)
    VibrationArray.setFrequency(buttonX, buttonY, 100Hz)
else:
    // Deactivate haptic feedback
    PeltierArray.setTemperature(buttonX, buttonY, ambientTemperature)
    VibrationArray.setFrequency(buttonX, buttonY, 0Hz)
```

**Refinements/Possibilities:**

*   **Dynamic Textures:** Create the sensation of different textures (e.g., wood grain, fabric) by rapidly modulating the thermal and vibration patterns.
*   **Shape Projection:** By controlling the thermal distribution, create the *illusion* of raised or recessed shapes on the back surface.
*   **Directional Feedback:** Use asymmetric thermal/vibration patterns to provide directional cues (e.g., a gentle “push” towards a specific edge).
*   **Gaming Applications:** Implement complex haptic feedback for gaming, simulating impacts, recoil, or environmental effects.
*    **Accessibility Features:** Provide tactile cues for visually impaired users.
*   **Biofeedback Integration:** Utilize the thermal sensors to detect skin temperature and adjust the haptic feedback accordingly, creating a more personalized experience.