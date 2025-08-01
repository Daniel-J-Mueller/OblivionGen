# D985453

## Dynamic Haptic Instrument Panel Overlay

**Concept:** Replace static visual elements of the instrument panel with a dynamically reconfigurable haptic surface. This surface would utilize an array of micro-actuators to create tactile feedback corresponding to information traditionally displayed visually.

**Specs:**

*   **Display Area:** Cover the primary instrument cluster area – approximately 300mm x 200mm.
*   **Actuator Density:** Minimum 100 actuators per 100mm², allowing for detailed tactile resolution. Piezoelectric actuators preferred for rapid response and low power consumption.
*   **Material:** A flexible, durable polymer substrate with integrated conductive traces for actuator control.  Material must withstand temperature fluctuations within automotive specifications (-40°C to +85°C).
*   **Control System:**  A dedicated microcontroller interfacing with the vehicle's CAN bus.  This microcontroller receives data points (speed, RPM, temperature, navigation prompts, etc.) and translates them into actuator commands.
*   **Tactile Library:** A pre-programmed library of tactile “shapes” and textures representing different information types:
    *   **Speed:**  A series of raised ridges, frequency modulating with speed – faster speed = faster ridge oscillation.
    *   **RPM:** A circular tactile "wave" emanating from the center, pulse rate corresponding to engine RPM.
    *   **Navigation:**  Directional bumps or grooves indicating turn direction. Intensity corresponds to proximity to turn.
    *   **Warnings:**  Sharp, localized tactile “pings” or vibrations.
    *   **Temperature:**  Variable texture – smooth for cool, rough for hot.
*   **User Customization:**  Software interface allowing users to customize the tactile representation of different data points.  Users should be able to assign specific tactile shapes to different vehicle functions.
*   **Integration with Existing Systems:** Compatibility with existing vehicle infotainment systems. Integration of voice commands for tactile feedback control (e.g. "Show me navigation tactile feedback").
*   **Power Requirements:**  12V DC, maximum 5A draw.  Implement a sleep mode to reduce power consumption when the vehicle is off.
*   **Safety Considerations:**  Actuators must be dampened to prevent distracting or startling vibrations. Tactile feedback should be complementary to visual displays, not a replacement.



**Pseudocode (Simplified Actuator Control):**

```
// Main Loop
while (vehicle_running) {

  // Read vehicle data from CAN bus
  speed = get_speed();
  rpm = get_rpm();
  navigation_direction = get_navigation_direction();
  warning_active = get_warning_status();

  // Calculate actuator patterns based on data
  speed_pattern = generate_speed_pattern(speed);
  rpm_pattern = generate_rpm_pattern(rpm);
  navigation_pattern = generate_navigation_pattern(navigation_direction);
  warning_pattern = generate_warning_pattern(warning_active);

  // Combine patterns (prioritize warnings)
  combined_pattern = merge_patterns(warning_pattern, navigation_pattern, rpm_pattern, speed_pattern);

  // Send actuator commands
  for (each actuator in actuator_array) {
    actuator.activate(combined_pattern[actuator.id]);
  }

  delay(10ms); // Update frequency
}
```