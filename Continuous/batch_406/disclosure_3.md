# 9552049

## Haptic Stylus with Variable Texture

**Concept:** A stylus incorporating the grip-switch technology, but extending it to actively modulate the tactile surface of the stylus itself, creating dynamic haptic feedback beyond simple activation.

**Specs:**

*   **Core Stylus Body:** Standard cylindrical stylus form factor, constructed from a rigid polymer.
*   **Grip-Switch Integration:** Incorporate the grip switch assembly described in the patent (compressible spacer, dual electrodes) as the primary activation mechanism.
*   **Micro-Actuator Array:** Surround the stylus body with an array of miniature piezoelectric actuators. These actuators will be embedded *under* a thin, flexible outer layer. The array density should be approximately 100 actuators per square centimeter.
*   **Outer Layer:** A thin (0.5mm), durable, and flexible polymer (e.g., TPU) coating the actuator array. This layer will be the user's primary tactile interface. Its surface will *not* be smooth. It will consist of microscopic, variable-height pillars.
*   **Actuator Control:** Each piezoelectric actuator will independently control the height of a corresponding pillar on the outer layer.
*   **Force Sensor Integration:** Integrate a secondary force sensor within the grip switch assembly to measure the *magnitude* of the grip force, not just its presence.
*   **Processing Unit:** A small, low-power microcontroller embedded within the stylus housing. Responsible for:
    *   Monitoring grip switch state (on/off, force magnitude).
    *   Controlling the actuator array.
    *   Receiving data from an external device (via Bluetooth/USB-C).
    *   Managing power consumption.
*   **Power Source:** Small rechargeable lithium-polymer battery.
*   **Communication:** Bluetooth 5.0 / USB-C interface.

**Operation:**

1.  **Activation:** Gripping the stylus activates the grip switch. The microcontroller transitions to an active state.
2.  **Force Modulation:** The magnitude of the grip force is measured.
3.  **Haptic Profile Selection:** The microcontroller receives a 'haptic profile' from the connected device. This profile defines the desired texture or feedback pattern for the stylus. Profiles could represent:
    *   Different writing surfaces (paper, canvas, metal).
    *   The edges of shapes or objects being drawn.
    *   Variations in line weight or shading.
    *   Unique textures for accessibility features.
4.  **Actuator Control:** The microcontroller maps the haptic profile to the actuator array. Each actuator is driven to a specific height, creating a dynamic texture on the stylus surface.
5.  **Real-time Adjustment:** The haptic profile can be adjusted in real-time based on user input, application state, or sensor data (e.g., drawing speed, pressure).
6.  **Passive Mode:** When the grip switch is deactivated, the actuators default to a neutral position, providing a smooth stylus surface.

**Pseudocode (Microcontroller):**

```
// Global Variables
bool stylusActive = false;
float gripForce = 0.0;
int[] hapticProfile;  // Array defining actuator heights
int numActuators = 100;

// Interrupt Service Routine (Grip Switch)
void gripSwitchISR() {
  if (digitalRead(gripSwitchPin) == HIGH) {
    stylusActive = true;
  } else {
    stylusActive = false;
  }
}

// Main Loop
void loop() {
  if (stylusActive) {
    gripForce = readForceSensor();

    //Receive haptic profile from external device (Bluetooth/USB)

    updateActuatorArray();  //Set actuator heights based on haptic profile

    sendStylusData(); //Send position, grip force, and other data
  }
  else {
    //Set all actuators to default height.
    resetActuatorArray();
  }
}
```

**Potential Applications:**

*   Digital art and illustration.
*   3D modeling and sculpting.
*   Accessibility features for visually impaired users.
*   Medical simulations and training.
*   Gaming and virtual reality.