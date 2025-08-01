# 9218020

## Modular Haptic Feedback System for Device Housings

**Concept:** Integrate a network of micro-actuators directly *into* the plastic housing of a computing device to provide localized haptic feedback, customizable alerts, and even simulated texture changes.

**Specifications:**

**1. Actuator Network:**

*   **Type:** Piezoelectric micro-actuators (stacked ceramic layers) – chosen for low power consumption and fast response time. Dimensions: 2mm x 2mm x 0.5mm.
*   **Density:** Variable, based on desired feedback resolution. Target: 1 actuator per 5cm² of housing surface. Higher density around frequently touched areas (edges, corners).
*   **Embedding:** Actuators encapsulated in a flexible, transparent polymer (TPU) and molded *directly into* the plastic housing during manufacturing. TPU acts as a vibration dampener *and* a protective layer.
*   **Addressing:** Each actuator assigned a unique address via a micro-wire grid embedded within the housing material.  Grid consists of insulated copper traces.
*   **Power:** Low-voltage DC (3-5V) supplied via the micro-wire grid.

**2. Control System:**

*   **Processor:** Dedicated low-power microcontroller within the device (integrated with existing sensor data – touch, proximity, accelerometer).
*   **Software:** API allowing developers to define haptic patterns & map them to device events. Includes pre-defined “haptic primitives” (pulse, ripple, texture).
*   **Mapping:** Haptic patterns dynamically mapped to specific areas of the housing. For example, a “slide to unlock” gesture could be accentuated by a ripple effect along the edge of the device.
*   **Customization:** User-adjustable haptic intensity and patterns.

**3. Housing Material Integration:**

*   **Material:** Polycarbonate/ABS blend with optimized flexibility to enhance actuator performance.
*   **Manufacturing:**  Two-shot molding process.  Actuators placed within a mold, then overmolded with the housing material.
*   **Wire Routing:** Micro-wire grid routed *within* the housing material during molding – protected from damage and visible from the exterior.

**4. Functionality Examples:**

*   **Notifications:**  Localized vibration pulses indicating incoming calls, messages, or low battery.
*   **UI Enhancement:** Simulating button presses, scroll wheel clicks, and other UI elements.
*   **Game Input:**  Providing immersive haptic feedback during gaming (e.g., simulating terrain textures, weapon recoil).
*   **Accessibility:**  Providing tactile cues for visually impaired users (e.g., marking important UI elements).
*    **Simulated Texture:** Program actuators to quickly oscillate, creating the illusion of texture on the housing surface.



**Pseudocode (Haptic Feedback Control):**

```
function generateHapticPattern(event, location, intensity, patternType) {
  // event: type of event triggering haptic feedback (e.g., "notification", "button_press")
  // location: x, y coordinates on the device housing
  // intensity: 0-100 (amplitude of vibration)
  // patternType:  "pulse", "ripple", "texture", "custom"

  if (patternType == "pulse") {
    actuatorArray[location].vibrate(intensity, 50ms) // Short pulse
  } else if (patternType == "ripple") {
    //Activate actuators in a circular pattern around location
    for (i = 0; i < radius; i++) {
      actuatorArray[location + i].vibrate(intensity * (1 - i/radius), 20ms)
    }
  } else if (patternType == "texture") {
    //Rapidly oscillate actuators to simulate texture
    for (i = 0; i < textureWidth; i++) {
      actuatorArray[location + i].oscillate(intensity, frequency)
    }
  } else if (patternType == "custom") {
    //Load and play a pre-defined custom haptic pattern
    playHapticPattern(patternID);
  }
}

function playHapticPattern(patternID) {
  //Retrieve pattern data from memory
  patternData = getHapticPattern(patternID);
  //Loop through pattern data and activate actuators accordingly
  for (i = 0; i < patternData.length; i++) {
    actuator = patternData[i].actuatorID;
    amplitude = patternData[i].amplitude;
    duration = patternData[i].duration;
    actuator.vibrate(amplitude, duration);
  }
}
```