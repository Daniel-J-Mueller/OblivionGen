# 9710087

## Haptic Light Field Display

**Concept:** Integrate the force-sensitive capacitive sensor stack with a micro-lens array and micro-LED array to create a haptic light field display. This allows for the creation of 3D imagery *and* localized tactile feedback, simulating texture and shape directly on the user’s fingertips.

**Specs:**

*   **Display Stack:**
    *   Cover Layer: High-transparency, durable polymer.
    *   Force-Sensitive Capacitive Sensor Stack: As described in the provided patent, utilizing optically clear silicon for the light guide. Modified to include a finer grid of capacitive sensors for increased spatial resolution of force detection.
    *   Micro-Lens Array: Positioned directly above the capacitive sensor stack. Each micro-lens focuses light from the micro-LED array onto a corresponding capacitive sensor and a point in space, forming a localized light field. Lens material: High refractive index polymer. Lens pitch: 50-200 microns.
    *   Micro-LED Array: High-density array of micro-LEDs positioned below the capacitive sensor stack. Each micro-LED corresponds to a micro-lens. Resolution: >10,000 PPI.
    *   Backplane: Thin-film transistor (TFT) backplane for individually addressing each micro-LED.
*   **Light Guide Modification:**
    *   Internal Reflective Structures: Integrate microscopic, angled reflective structures *within* the optically clear silicon light guide. These structures direct light towards the micro-lens array, maximizing brightness and minimizing light loss. Structures designed via nano-imprint lithography.
    *   Variable Light Guide Thickness: Implement a spatially varying thickness of the light guide, allowing for localized control of light intensity and brightness.
*   **Haptic Feedback Algorithm:**
    *   Force Mapping: Real-time algorithm maps detected forces on the capacitive sensor stack to corresponding haptic feedback patterns.
    *   Micro-LED Dimming Control: The algorithm adjusts the brightness of the micro-LEDs corresponding to the detected force. Dimming creates the illusion of texture and shape by modulating light intensity.
    *   Dynamic Resolution Scaling: Adjusts the spatial resolution of the haptic feedback based on user proximity and interaction.
*   **System Architecture:**
    *   High-Speed Data Processing Unit: Dedicated processor for real-time force detection, haptic feedback calculation, and micro-LED control.
    *   Communication Interface: High-bandwidth interface for receiving 3D image data and haptic feedback commands.
    *   Power Management System: Efficient power supply for the entire display stack.

**Pseudocode (Haptic Feedback Algorithm):**

```
function generateHapticFeedback(forceData, microLEDArray):
    // forceData: 2D array representing force readings from capacitive sensor stack
    // microLEDArray: 2D array representing micro-LEDs

    for i in range(rows(forceData)):
        for j in range(columns(forceData)):
            forceValue = forceData[i][j]

            // Map force value to brightness level (0-100%)
            brightness = map(forceValue, 0, maxForce, 0, 100)

            // Apply brightness to corresponding micro-LED
            microLEDArray[i][j].setBrightness(brightness)

    return microLEDArray
```

**Novelty:** Combines force sensing with a light field display to create a truly immersive and interactive experience. Users can *feel* the 3D imagery, enhancing realism and usability for applications like gaming, virtual reality, medical imaging, and design.