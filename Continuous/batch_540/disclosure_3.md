# D945898

## Adaptive Bio-Luminescence Doorbell

**Concept:** A doorbell incorporating bioluminescent fungal mycelium within a transparent, protective housing. The doorbell doesn't *ring* traditionally; instead, the mycelium network glows with increasing intensity and patterns responsive to both button presses *and* approaching individuals detected via low-resolution thermal imaging. 

**Specs:**

*   **Housing:** Spherical, approximately 15cm diameter. Constructed from clear, impact-resistant acrylic with UV filtering. Internal structure includes a lattice to support and aerate mycelial growth. Sealed, but with a humidity control valve.
*   **Mycelium:** *Mycena luxaeterna* or similar bioluminescent fungal species. Genetically modified (optional) to enhance luminescence and broaden emission spectrum.  Grown on a substrate of sterilized wood chips and nutrients within the housing.  Substrate replaced/supplemented via access port.
*   **Sensor Suite:**
    *   Low-resolution (8x8 pixel) thermal sensor – detects heat signatures. Range: 3-5 meters.
    *   Capacitive touch sensor – Button press detection.
    *   Ambient Light Sensor – Adjusts luminescence intensity for optimal visibility.
*   **Control System:** Microcontroller (ESP32 or similar).  Receives input from sensors.  Controls luminescence via modulating nutrient delivery to the mycelium.
*   **Luminescence Modulation:**
    *   Button Press:  Pulse of bright, blue-green light, spreading outwards from the center.
    *   Approaching Person (Thermal):  Increasingly bright, radial glow, color shifting from green to orange as proximity increases.  Pattern differentiation (distinct glow patterns for different detected individuals – potentially through size/heat signature analysis – for 'guest recognition').
*   **Power:** Wireless charging (Qi standard) or low-voltage DC adapter. Internal battery backup.
*   **Humidity Control:** Small dehumidifier module. Microcontroller adjusts humidity based on internal sensors and external temperature.
*   **Nutrient Delivery:**  Micro-peristaltic pump delivering liquid nutrient solution to the mycelial substrate.  Pump controlled by microcontroller based on luminescence intensity and mycelial growth rate (estimated from luminescence levels).
*   **Self-Repair:**  The mycelial network naturally self-repairs minor damage to the glow.



**Pseudocode (Luminescence Control):**

```
// Global Variables
float thermal_distance = 0;
bool button_pressed = false;
int luminescence_level = 0;

// Function: Process Thermal Data
function processThermalData() {
  thermal_distance = readThermalSensor();
  if (thermal_distance < 3.0) {
    luminescence_level = map(thermal_distance, 0, 3, 0, 255);
    setLuminescence(luminescence_level);
  } else {
    setLuminescence(0);
  }
}

// Function: Handle Button Press
function handleButtonPress() {
  if (readButtonSensor() == true) {
    // Pulse effect
    for (int i = 0; i < 5; i++) {
      setLuminescence(255);
      delay(50);
      setLuminescence(0);
      delay(50);
    }
  }
}

// Function: Adjust Ambient Light
function adjustAmbientLight() {
  ambient_light = readAmbientLightSensor();
  // Adjust luminescence level based on ambient light
}

// Main Loop
loop() {
  processThermalData();
  handleButtonPress();
  adjustAmbientLight();
}
```