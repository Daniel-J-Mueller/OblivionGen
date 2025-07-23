# D678049

## Kinetic Chamfered Box - "Bloom"

**Concept:** A chamfered box featuring dynamically adjustable external facets controlled by internal actuators. The box's overall form remains recognizably a chamfered box, but the specific angles and arrangements of the chamfered surfaces *change* over time, creating a subtly animated or reactive external appearance. This is achieved through small, precise movements of internal panels, essentially "blooming" or shifting the external geometry.

**Materials:**

*   **Core Structure:** High-density polymer (ABS or similar) providing a rigid base.
*   **Actuator Mechanism:** Miniature linear actuators (piezoelectric or micro-servo) - approximately 5-10mm in length.
*   **Facet Panels:** Thin, lightweight panels – potentially a flexible polymer or a metallic alloy with a polished finish.
*   **Control System:** Microcontroller (ESP32 or similar) with wireless communication (Bluetooth/Wi-Fi).
*   **Power:** Rechargeable Lithium Polymer battery.

**Construction:**

1.  **Internal Framework:** The core structure will house a multi-layered internal framework. This framework contains mounting points for the actuators and the facet panels.
2.  **Facet Panel Integration:** Each facet panel is attached to at least one (potentially two) actuators. The actuators are strategically positioned to allow for a defined range of motion - tilting, extending, or retracting the panel.
3.  **Actuator Placement:** Actuators are placed behind each facet, connecting to the panel via a small, low-friction linkage. The linkages translate the linear motion of the actuator into the desired angular or linear movement of the panel.
4.  **Control System Integration:** The microcontroller is connected to each actuator, allowing for individual control of each facet panel.
5.  **Power Supply:** The battery is integrated within the core structure and provides power to the microcontroller and actuators.

**Functionality/Pseudocode:**

```
// Define number of facets
int numFacets = 8;

// Define actuator pins for each facet
int actuatorPins[numFacets] = {2, 3, 4, 5, 6, 7, 8, 9};

// Define minimum and maximum angles for each facet (in degrees)
int minAngle[numFacets] = {5, 10, 15, 20, 25, 30, 35, 40};
int maxAngle[numFacets] = {45, 50, 55, 60, 65, 70, 75, 80};

// Function to map angle to actuator position
int mapAngleToActuator(int angle, int minAngle, int maxAngle) {
  // Assuming actuator has a range of 0-1023
  return map(angle, minAngle, maxAngle, 0, 1023);
}

void setup() {
  // Initialize actuator pins as output
  for (int i = 0; i < numFacets; i++) {
    pinMode(actuatorPins[i], OUTPUT);
  }
}

void loop() {
  // Cycle through facets
  for (int i = 0; i < numFacets; i++) {
    // Generate a smooth, oscillating angle
    float angle = sin(millis() / 1000.0 + i * 0.5) * 10 + 30; // Adjust parameters as needed

    // Constrain angle within min/max range
    angle = constrain(angle, minAngle[i], maxAngle[i]);

    // Map angle to actuator position
    int actuatorPosition = mapAngleToActuator(angle, minAngle[i], maxAngle[i]);

    // Send signal to actuator
    analogWrite(actuatorPins[i], actuatorPosition);

    // Delay for smooth animation
    delay(10);
  }
}
```

**Variations/Further Development:**

*   **Reactive Surface:** Integrate sensors (light, sound, touch) to trigger changes in facet angles.
*   **Programmable Patterns:** Allow users to define custom animation sequences via a mobile app.
*   **Material Exploration:** Experiment with different materials for facet panels (e.g., dichroic films, electrochromic materials) to create dynamic visual effects.
*   **Internal Lighting:** Integrate LEDs within the core structure to illuminate the changing facets.
*   **Haptic Feedback:** Incorporate small vibration motors to provide tactile feedback when facets shift.