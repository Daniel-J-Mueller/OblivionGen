# D918212

## Modular, Kinetic Cover System

**Concept:** A phone cover comprised of numerous small, independently articulating tiles, allowing for dynamic surface texture and user-defined patterns/animations.

**Materials:**

*   **Tile Base:** Thermoplastic Polyurethane (TPU) - provides flexibility and impact resistance. Dimensions: 8mm x 8mm x 1.5mm.
*   **Actuators:** Micro-electromechanical systems (MEMS) linear actuators embedded within each tile. Power draw: <5mW per actuator. Range of motion: 0.5mm.
*   **Surface Material:**  E-Ink or micro-LED array on the top surface of each tile. Resolution: 64x64 pixels per tile.
*   **Connectivity:** Wireless charging and data transfer via NFC/Bluetooth Low Energy.
*   **Core:** Rigid internal frame (Polycarbonate) for structural integrity, housing the battery and control electronics.

**Functionality:**

1.  **Tile Arrangement:**  Tiles are connected to the core frame via miniature ball-and-socket joints, allowing for individual movement. Density: 100 tiles per standard phone size.
2.  **Kinetic Texture:** The MEMS actuators raise or lower individual tiles, creating dynamic textures. Example: raised bumps for grip, recessed areas for a smooth surface, Braille patterns.
3.  **Visual Display:** The micro-LED or E-Ink displays on each tile can create images, animations, and custom patterns. The tiles function as a low-resolution, flexible display.
4.  **Haptic Feedback:** Tile movement and texture changes provide localized haptic feedback.
5.  **User Control:** A companion app allows users to:
    *   Design custom textures and animations.
    *   Assign different textures/animations to specific app notifications.
    *   Create patterns based on incoming calls/messages.
    *   Control tile movement speed and intensity.

**Pseudocode (Animation Control):**

```
// Function: createWaveAnimation
// Parameters: amplitude (float), frequency (float), duration (int)
// Description: Creates a wave-like animation across the tile array.

function createWaveAnimation(amplitude, frequency, duration) {
  for (tile in tileArray) {
    timeOffset = tile.x * frequency; // Calculate offset based on tile position
    yOffset = amplitude * sin(time + timeOffset); // Calculate vertical offset
    tile.move(0, yOffset); // Move the tile
    delay(10ms);
  }
}

// Function: displayNotificationPattern
// Parameters: notificationType (string)
// Description: Displays a predefined pattern on the tile array based on the notification type.

function displayNotificationPattern(notificationType) {
  if (notificationType == "call") {
    // Display a pulsing pattern
    for (tile in tileArray) {
      tile.setColor(red);
      delay(200ms);
      tile.setColor(black);
      delay(200ms);
    }
  } else if (notificationType == "message") {
    // Display a scrolling pattern
    for (tile in tileArray) {
      tile.setColor(blue);
      delay(50ms);
      tile.setColor(black);
    }
  }
}

// Event Listener: OnNotificationReceived
// Description: Triggers the displayNotificationPattern function based on the received notification.

onNotificationReceived(notification) {
  notificationType = notification.type;
  displayNotificationPattern(notificationType);
}
```

**Power Management:**

*   Wireless charging.
*   Low-power MEMS actuators.
*   Optimized E-Ink/micro-LED display operation.
*   Sleep mode when inactive. Estimated battery life: 24 hours with moderate use.

**Potential Enhancements:**

*   Solar charging integration.
*   Gesture control for animation selection.
*   Integration with virtual assistant (Siri, Google Assistant).
*   Customizable tile materials and colors.
*   Pressure sensitivity on tiles for gaming/input.