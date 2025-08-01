# 10100553

## Mechanical Keyway 'Fingerprint' System

**Concept:** Expand upon the idea of detecting foreign objects in the keyway, but move beyond simple obstruction detection. Instead, implement a system that analyzes the *shape* of the object inserted into the keyway, creating a mechanical ‘fingerprint’. This could differentiate between authorized keys, picking tools, bumped keys, and even attempts to create molds of the keyway.

**Specs:**

*   **Keyway Liner:** The internal surface of the keyway will be lined with a series of miniature, spring-loaded pins (approx. 1-2mm in length). These pins will be arranged in a dense grid along the entire length of the keyway.
*   **Pin Array Sensors:** Each pin will be linked to a miniature, magnetically-coupled lever arm.  The position of each lever arm will be read by a series of Hall-effect sensors positioned around the keyway. This creates a 'map' of the object inserted.
*   **Authorized Key Profile:**  An authorized key will have corresponding recesses designed to *depress* specific pins, creating a unique signature within the Hall-effect sensor array. 
*   **Tamper Detection Logic:** A microcontroller will be programmed to compare the Hall-effect sensor readings to a pre-programmed database of authorized key signatures. Deviations from these signatures trigger tamper detection.
*   **Tamper Response:** Tamper detection can initiate several responses:
    *   Lockout (as in the source patent).
    *   Alarm activation.
    *   Recording of the sensor data for forensic analysis.
*   **Mechanical Indexing for Resilience:** A secondary, purely mechanical indexing system will be used to record tamper *attempts* regardless of power. This involves a ratchet and pawl mechanism linked to the primary pin array. Each tamper event advances the ratchet, and a predetermined number of advancements triggers a permanent lockout.
*   **Self-Calibration:** The system will include a self-calibration routine. Upon insertion of a known-good key, the system will re-establish the baseline pin positions and calibrate the sensor array.

**Pseudocode (Tamper Detection):**

```
// Initialize sensor array and baseline key profile
load_baseline_profile()

// Read sensor data upon key insertion
sensor_data = read_sensor_array()

// Compare sensor data to baseline profile
similarity_score = compare_profiles(sensor_data, baseline_profile)

if (similarity_score < threshold) {
  // Tamper detected!
  record_tamper_event()
  initiate_lockout()
  activate_alarm()
} else {
  // Authorized key – allow rotation
  allow_rotation()
}
```

**Materials:**

*   High-strength steel for keyway and internal components.
*   Miniature Hall-effect sensors and associated circuitry.
*   Spring steel for pin mechanisms.
*   Magnetic shielding materials to minimize interference.