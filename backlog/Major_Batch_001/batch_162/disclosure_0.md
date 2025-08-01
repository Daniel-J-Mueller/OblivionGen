# 10120423

## Adaptive Phase Change Material Integration

**Concept:** A thermal enclosure incorporating micro-capsulated Phase Change Material (PCM) within the structural material, dynamically adjusting thermal conductivity based on localized heat generation.

**Specs:**

*   **Structural Material:** Thermoplastic polyurethane (TPU) matrix.  Chosen for flexibility and ease of micro-capsule dispersion.
*   **PCM:**  Micro-encapsulated Rubidium Chloride (RuCl3) – exhibits a phase change temperature around 45°C. Capsule diameter: 50-100 μm. Concentration: 10-20% by weight within the TPU.
*   **Heat Source Mapping:**  Integrated thermal diode array (1mm pitch) within the enclosure, directly contacting the electronic components.  Data streamed to a micro-controller.
*   **Micro-controller:**  ESP32-S3.  Processes thermal diode data, and modulates the power to localized resistive heating elements embedded *within* the TPU matrix surrounding high-heat components.
*   **Resistive Heating Elements:** Graphene ink traces, 50 μm thick, 200 μm wide.  Precisely positioned around predicted heat hotspots based on component specifications. Controlled via PWM signal from the ESP32-S3.
*   **Enclosure Layering:**
    *   **Exterior:**  Thermally conductive polymer (e.g., carbon fiber reinforced polymer) for radiative heat dissipation.
    *   **Mid-Layer:** TPU/PCM composite. This layer actively manages heat flux.
    *   **Interior:**  Thin layer of thermally interface material (TIM) for maximizing heat transfer to/from components.
*   **Power:** USB-C PD for both data communication and power supply to the micro-controller and resistive heating elements.

**Operation:**

1.  The thermal diode array continuously monitors component temperatures.
2.  The micro-controller analyzes the data and identifies hotspots.
3.  When a hotspot exceeds a threshold, the micro-controller activates the corresponding resistive heating element.
4.  The localized heating triggers the PCM in that area to undergo a phase change (solid to liquid), *increasing* thermal conductivity. This draws heat *away* from the hotspot and spreads it more evenly throughout the enclosure.
5.  As the temperature decreases, the PCM re-solidifies, reducing thermal conductivity.
6.  The process repeats dynamically, maintaining a more uniform temperature distribution and preventing localized overheating.

**Pseudocode:**

```
// Define Temperature Thresholds
const float TEMP_THRESHOLD_HIGH = 60.0;  // Degrees Celsius
const float TEMP_THRESHOLD_LOW = 50.0;   // Degrees Celsius

// Define PWM Range
const int PWM_MAX = 255;

// Thermal Diode Reading Function
float readThermalDiode(int diodePin) {
  // Analog read, scale to temperature based on diode characteristics
  // Return temperature in Celsius
}

// PWM Control Function
void setPWM(int pwmPin, int dutyCycle) {
  // Set PWM duty cycle for a given pin
}

// Main Loop
void loop() {
  for (int i = 0; i < NUM_DIODES; i++) {
    float temperature = readThermalDiode(diodePins[i]);

    if (temperature > TEMP_THRESHOLD_HIGH) {
      // Activate corresponding heating element
      setPWM(heatingElementPins[i], PWM_MAX);
    } else if (temperature < TEMP_THRESHOLD_LOW) {
      // Deactivate heating element
      setPWM(heatingElementPins[i], 0);
    }
  }
}
```

**Novelty:**  Traditional thermal enclosures rely on passive heat dissipation. This design actively *manages* heat flux using a dynamic thermal conductivity material and localized heating elements, creating a self-regulating thermal system. The use of micro-encapsulated PCM within the structural material is a significant departure from existing thermal management techniques.