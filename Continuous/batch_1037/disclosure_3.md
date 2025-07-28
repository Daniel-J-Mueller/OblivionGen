# D1010313

## Adaptive Haptic Feedback Earbud Case

**Concept:** An earbud case incorporating localized haptic feedback to indicate charging status, battery level, incoming calls/notifications, or even act as a mini-game interface.

**Specifications:**

*   **Case Material:** Primarily rigid polycarbonate with localized flexible TPU sections.
*   **Haptic Actuators:** Array of miniature linear resonant actuators (LRAs) embedded within the TPU sections of the case. Density: 16 actuators distributed across the top and sides.
*   **Power Source:**  Integrated inductive charging coil capable of receiving power from a standard Qi charger.  A small, dedicated battery within the case powers the haptic actuators - separate from the earbuds' charging battery. Capacity: 50 mAh.
*   **Microcontroller:** Low-power ARM Cortex-M0 microcontroller to manage charging, haptic patterns, and Bluetooth communication.
*   **Bluetooth Communication:** Bluetooth Low Energy (BLE) module for communication with the paired smartphone.
*   **Software/Firmware:**
    *   **Charging Indication:** Gradual increase in haptic intensity on the case top to represent charging progress. Full charge indicated by a distinct, sustained vibration.
    *   **Battery Level Indication:**  Number of sequentially activated LRAs on the side of the case represents the remaining battery percentage.  (e.g., 10 LRAs activated = 10% battery).
    *   **Notification Mapping:** User-configurable haptic patterns for different types of notifications (calls, texts, emails, app alerts).
    *   **"Mini-Game" Mode:**  A simple reaction-based game playable via the case itself.  LRAs activate in a sequence, and the user taps the case to "catch" them. Score tracked via BLE.
*   **Sensor Integration:**
    *   **Tap Sensor:** Piezoelectric sensor embedded in the case top to detect user taps for game interaction or control.
    *   **Proximity Sensor:** Detects when the case is opened/closed and adjusts haptic behavior accordingly (e.g., silences notifications when open).

**Pseudocode (Notification Handling):**

```
function handleNotification(notificationType, data) {
  if (notificationType == "incomingCall") {
    playHapticPattern("pulse", 0.2, 3);  // Short, rapid pulses
  } else if (notificationType == "textMessage") {
    playHapticPattern("buzz", 0.1, 1);  // Single, short buzz
  } else if (notificationType == "email") {
    playHapticPattern("wave", 0.05, 2); // Two quick, descending vibrations
  } else if (notificationType == "appAlert") {
    // Retrieve custom haptic pattern from user preferences
    customPattern = getUserPreference("appAlertPattern");
    playHapticPattern(customPattern);
  }
}

function playHapticPattern(patternName, intensity, repetitions) {
  // Look up pattern in haptic library
  pattern = getHapticPattern(patternName);
  // Activate LRAs according to pattern
  for (i = 0; i < pattern.length; i++) {
    actuateLRA(pattern[i], intensity);
    delay(pattern[i].duration);
  }
  // Repeat pattern as needed
  for (j = 1; j < repetitions; j++) {
    // Same as above
  }
}
```

**Further Considerations:**

*   Integration with voice assistants (Siri, Google Assistant) for control.
*   Customizable vibration intensity levels.
*   User-defined haptic patterns via a smartphone app.
*   Potential for gesture control (e.g., swipe on the case to dismiss a notification).