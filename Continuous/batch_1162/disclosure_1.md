# D1017105

## Adaptive Bioluminescence Night Light

**Concept:** A night light mimicking bioluminescent organisms, dynamically adjusting brightness and color based on ambient sound and user biofeedback.

**Specs:**

*   **Form Factor:** Spherical housing, approximately 15cm diameter, constructed from translucent, frosted acrylic. Internal cavity houses all electronics and light-emitting components.
*   **Light Source:** Utilize a combination of micro-LEDs and a bio-luminescent material (engineered bacteria or synthetic analog) suspended in a clear, viscous gel within the acrylic sphere. LEDs serve as initial "trigger" and color modulation, while bioluminescent material provides a softer, more organic glow.
*   **Sound Sensor:** High-sensitivity microphone array (3-5 mics) integrated into the base of the sphere. Frequency range: 20Hz - 20kHz.  Purpose: Detect ambient sound levels and specific sound signatures (e.g., baby crying, speech, music).
*   **Biofeedback Sensor:** Integrated capacitive touch sensor on the sphere's surface.  Detects skin conductance (GSR - Galvanic Skin Response) when touched. Also incorporates a heart rate sensor using photoplethysmography (PPG).
*   **Microcontroller:** ESP32-S3 with integrated Wi-Fi and Bluetooth. Responsible for processing sensor data, controlling LEDs, and managing bioluminescence.
*   **Power Source:** Rechargeable lithium-ion battery (3.7V, 2000mAh) with wireless charging capability (Qi standard).
*   **Software/Algorithm:**
    *   **Ambient Sound Response:**
        *   Algorithm analyzes ambient sound levels.
        *   Below 30dB: Dim, calming blue/green bioluminescence.
        *   30-60dB: Increase bioluminescence intensity.  Introduce subtle color shifts (e.g., towards warmer tones).
        *   Above 60dB: Rapidly increase bioluminescence to maximum intensity, switch to brighter white/yellow light for a short duration (alert mode).
        *   Specific Sound Detection:  Baby cry detection triggers a soft, pulsating red light. Speech detection activates a slightly brighter, warmer light. Music detection initiates a dynamic color-changing mode synchronized to the beat.
    *   **Biofeedback Response:**
        *   GSR Monitoring: Increase light intensity and shift towards calming colors (blue/green) if GSR indicates stress or anxiety.
        *   Heart Rate Monitoring: Slow, rhythmic pulsing of light synchronized to the user’s heartbeat to promote relaxation.
    *   **Adaptive Learning:** Implement a machine learning algorithm (e.g., neural network) to learn user preferences and optimize light settings based on collected data (sound, biofeedback, user input).
*   **User Interface:** Minimalist touch controls on the sphere’s surface for manual brightness adjustment and mode selection.  Optional mobile app connectivity for advanced customization and data visualization.
*   **Safety Features:** Overheat protection, short-circuit protection, automatic shutdown if malfunction detected. Bioluminescent material encapsulated in a non-toxic, sealed gel.

**Pseudocode:**

```
// Main loop
while (true) {
  soundLevel = readSoundSensor();
  gsrValue = readGSRSensor();
  heartRate = readHeartRateSensor();

  if (soundLevel < 30) {
    setLEDColor(blue);
    setLEDIntensity(low);
    activateBioluminescence(low);
  } else if (soundLevel >= 30 && soundLevel < 60) {
    setLEDColor(green);
    setLEDIntensity(medium);
    activateBioluminescence(medium);
  } else {
    setLEDColor(white);
    setLEDIntensity(high);
    activateBioluminescence(high);
  }

  if (gsrValue > threshold) {
    setLEDColor(blue);
    setLEDIntensity(low);
  }

  if (heartRate > threshold) {
    pulseLight(heartRate);
  }
}
```