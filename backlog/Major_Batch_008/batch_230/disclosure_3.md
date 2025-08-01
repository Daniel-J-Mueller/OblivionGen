# D909993

## Dynamic Haptic Texture Mapping

**Concept:** An electronic device incorporating a dynamic haptic surface capable of rendering textures *on demand* through localized thermal variation coupled with micro-vibration.  This goes beyond simple vibration; it aims to *simulate* the feeling of different materials and surfaces.

**Specs:**

*   **Display Integration:**  The device features a layered construction. The outermost layer is a flexible, durable polymer (e.g., TPU) serving as the haptic surface. Beneath this is a matrix of individually addressable micro-heaters (Peltier elements could also be employed for both heating *and* cooling). Below the heaters is an array of micro-actuators (piezoelectric or micro-EM motors) capable of high-frequency, localized vibration.
*   **Thermal Resolution:**  Heater matrix resolution: 64x64 micro-heaters per square inch.  Individual heater control range: 20°C – 60°C.  Response time: < 50ms per heater.
*   **Vibration Resolution:** Actuator matrix resolution: 128x128 per square inch. Frequency range: 20Hz – 2kHz. Amplitude control: 0 – 2mm displacement.  Precise control of phase and amplitude for each actuator.
*   **Surface Material:** The flexible polymer surface will be treated with a micro-texture coating to enhance the perceived textural differences. Coatings could include variations in roughness, friction coefficient, and thermal conductivity.
*   **Control System:**  A dedicated processor will manage the thermal and vibrational output. The processor will receive texture map data (described below) and translate it into specific heater and actuator commands.
*   **Texture Map Data:** Texture maps will be composed of two primary data layers:
    *   **Thermal Map:**  A grayscale image representing the desired temperature distribution across the surface.  Brighter pixels indicate higher temperatures.
    *   **Vibration Map:**  A multi-channel image representing the frequency and amplitude of vibration for each point on the surface. Channels: Frequency, Amplitude, Phase.
*   **Software Interface:**  API to allow developers to create and integrate dynamic haptic textures into applications. Includes tools for generating and editing texture maps.
*   **Power Requirements:** < 5W during typical operation.

**Innovation Detail:**

The combination of localized heating/cooling *and* high-frequency vibration allows for surprisingly realistic texture simulation. For example:

*   **Simulating Roughness:**  Rapidly cycle heating and cooling in small areas combined with high-frequency vibration to create the *illusion* of a rough surface.
*   **Simulating Stickiness:**  Localized warming combined with a low-frequency vibration and a slightly higher friction surface coating.
*   **Simulating Coldness:**  Localized cooling combined with minimal vibration.
*   **Material Differentiation:** The thermal map can simulate the temperature properties of different materials (e.g., metal feels colder than wood).

**Pseudocode (Texture Rendering):**

```
function RenderTexture(thermalMap, vibrationMap) {
  for (x = 0; x < thermalMap.width; x++) {
    for (y = 0; y < thermalMap.height; y++) {
      temperature = thermalMap.getPixel(x, y);
      vibrationFrequency = vibrationMap.getFrequency(x, y);
      vibrationAmplitude = vibrationMap.getAmplitude(x, y);
      vibrationPhase = vibrationMap.getPhase(x,y);

      setHeaterTemperature(x, y, temperature);
      setActuatorFrequency(x, y, vibrationFrequency);
      setActuatorAmplitude(x, y, vibrationAmplitude);
      setActuatorPhase(x,y, vibrationPhase);
    }
  }
}
```