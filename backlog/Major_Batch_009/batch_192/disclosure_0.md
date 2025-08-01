# D902471

## Adaptive Bioluminescence Night Light

**Concept:** A night light mimicking natural bioluminescence, dynamically adjusting intensity and color based on detected ambient sound and room occupancy, creating a calming and responsive atmosphere.

**Specs:**

*   **Core:** Microcontroller (ESP32 preferred) with integrated Wi-Fi/Bluetooth.
*   **Light Source:** Array of individually addressable RGB LEDs (WS2812B or similar) diffused by a sculpted, organic-shaped light diffuser (bioplastic preferred). The diffuser shape will be inspired by jellyfish or deep-sea creatures.
*   **Sound Sensor:** MEMS microphone module (e.g., MAX9814) to detect ambient sound levels and frequencies.
*   **Occupancy Sensor:** Passive Infrared (PIR) sensor to detect room occupancy. Alternatively, a low-resolution time-of-flight (ToF) sensor for more accurate presence detection and distance measurement.
*   **Power:** USB-C powered.
*   **Enclosure:**  3D-printed bioplastic enclosure, designed to resemble a natural form (e.g., smooth pebble, stylized mushroom).

**Functionality:**

1.  **Base State:** When no sound or occupancy is detected, the light emits a very dim, cool-toned blue-green glow, simulating deep-sea bioluminescence.  LEDs cycle subtly through shades to avoid a static appearance.

2.  **Sound Response:**
    *   Sound level triggers intensity adjustment. Louder sounds (e.g., baby crying, sudden noise) cause a brighter, warmer (yellow/orange) pulse.  Softer sounds (e.g., whispering, gentle music) result in a dimmer, cooler-toned glow.
    *   Frequency analysis:  Different frequency ranges trigger different color shifts.  Higher frequencies (e.g., bird song) could trigger brief flashes of turquoise. Lower frequencies (e.g., heartbeat-like sounds) trigger a slow pulsing amber.
    *   Pseudocode:

```
function onSoundDetected(soundLevel, frequency) {
  if (soundLevel > thresholdHigh) {
    setLEDColor(orange);
    setLEDIntensity(bright);
    pulseLED(duration=500ms);
  } else if (soundLevel > thresholdMedium) {
    setLEDColor(paleYellow);
    setLEDIntensity(medium);
  } else {
    setLEDColor(blueGreen);
    setLEDIntensity(dim);
  }

  if (frequency > frequencyHigh) {
    flashLED(color=turquoise, duration=100ms);
  } else if (frequency < frequencyLow) {
    slowPulseLED(color=amber, period=2s);
  }
}
```

3.  **Occupancy Response:**
    *   When occupancy is detected, the light gradually increases in brightness from its base state.
    *   If occupancy is absent for a defined period (e.g., 30 minutes), the light returns to its base state.
    *   ToF sensor integration: If the ToF sensor detects a person approaching, the light could pre-brighten slightly, creating a welcoming effect.

4.  **Connectivity (Optional):**
    *   Wi-Fi connectivity allows for remote control via a mobile app (brightness, color, sound sensitivity).
    *   Integration with smart home platforms (e.g., Alexa, Google Home) for voice control.
    *   Data logging of sound levels and occupancy patterns (for potential insights into sleep patterns – privacy considerations crucial).

5.  **Color Palette:** Focus on cool, calming colors (blues, greens, violets) with subtle shifts in warmer tones to indicate activity. Avoid harsh, bright colors.

6.  **Material:** Utilize sustainable and biodegradable materials whenever possible (e.g., bioplastic enclosure, plant-based diffuser).