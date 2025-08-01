# 12200424

## Stacked Haptic & Thermal Array

**Concept:** Expand the capacitive touch sensing system into a multi-dimensional interface incorporating localized thermal control. This creates a stacked array capable of both touch *and* temperature feedback, increasing immersion and usability, particularly in wearable applications.

**Specifications:**

*   **Array Architecture:** A matrix arrangement of capacitive touch sensors combined with micro-Peltier elements (thermoelectric coolers/heaters).  The matrix will be layered – capacitive sensor layer, insulating layer, Peltier element layer.

*   **Sensor Density:** Target 200+ capacitive sensors/Peltier elements per square centimeter.  Miniaturization of both components is crucial.

*   **Flexible Substrate:** Utilize a multi-layer flexible printed circuit (FPC) as the base. This allows for conformal application to curved surfaces. The FPC will incorporate dedicated traces for capacitive sensing, Peltier element power, and control signals.

*   **Peltier Element Control:** Each Peltier element will have independent PWM (Pulse Width Modulation) control to manage temperature.  A closed-loop temperature regulation system (thermistor-based feedback) will maintain precise control.

*   **Capacitive Sensing Integration:** Integrate the capacitive touch sensors directly with the PWM control system.  A touch event will not only register a location, but *also* trigger a localized thermal response (e.g., warming a specific point, creating a 'heat signature' for feedback).

*   **Power Management:** Implement a dynamic power allocation scheme.  Reduce power to inactive Peltier elements to conserve battery life. The button cell battery from the original patent will supply power.

*   **Insulation Layer:** A thin layer of thermally insulating material (e.g., aerogel, polyimide) will separate the capacitive sensors from the Peltier elements, preventing cross-interference and maximizing thermal efficiency.

*   **Control Logic (Pseudocode):**

```
//Main Loop

while (running) {

    //Read Capacitive Touch Sensor Array
    touchData = readTouchArray();

    //Read Temperature Sensor Array
    tempData = readTempArray();

    //Process Touch Events
    for (each touch event in touchData) {
        location = touchEvent.location;
        // Trigger thermal response at location
        setTemperature(location, desiredTemperature);
    }

    //Thermal Regulation Loop
    for (each element in tempArray) {
      if (element.temperature != desiredTemperature) {
        adjustPeltierElement(element, desiredTemperature);
      }
    }
}
```

*   **Material Specifications:**

    *   Flexible PCB Material: Polyimide
    *   Capacitive Sensor Electrode Material: Silver Nanowires / ITO
    *   Peltier Element Material: Bismuth Telluride
    *   Insulating Layer: Aerogel / Polyimide Film
* **Stacking Order:**

    1.  Flexible PCB (Bottom Layer – Signal Routing)
    2.  Capacitive Sensor Array
    3.  Insulating Layer
    4.  Peltier Element Array
    5.  Protective Overcoat (Polyurethane/Epoxy) – For environmental protection and mechanical stability.

* **Applications:**

    *   Advanced Haptic Feedback in VR/AR wearables.
    *   Localized Thermal Control in smart clothing (e.g., temperature regulation for athletic performance).
    *   Intuitive user interfaces for wearable devices (e.g., virtual buttons with thermal confirmation).