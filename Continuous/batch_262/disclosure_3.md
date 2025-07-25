# 9013416

## Dynamic Haptic Feedback Across Dual Displays

**Concept:** Extend the touch sensitivity of the dual-display device to include localized haptic feedback, not just *on* the displays themselves, but *between* them, utilizing the casing as a conduit for localized vibrations. This creates a more immersive and intuitive user experience for content manipulation and interaction.

**Specs:**

*   **Haptic Actuator Array:** Integrate a dense array of micro-actuators (piezoelectric or similar) *within* the device casing, strategically positioned along the edges and potentially extending onto the rear surface. Resolution: minimum 20 actuators per inch.
*   **Force-Sensitive Interlayer:** Place a flexible, force-sensitive resistive material between the displays and the actuator array. This allows the system to detect the *position and pressure* of a user’s grip or touch even when not directly contacting a display.
*   **Content Aware Haptic Engine:** Develop software that dynamically maps content on either display to specific haptic patterns delivered through the actuator array.
    *   **Dragging/Moving Content:** When a user drags content from one display to the other (as described in the patent), the actuator array creates a localized 'pull' sensation in the direction of movement, mimicking a physical dragging effect. Vary the intensity based on speed and content weight (size/complexity).
    *   **Edge-Based Interactions:** Allow users to interact with content *by* touching the casing edges near a display. For instance, ‘sliding’ a finger along the edge could scroll content on the adjacent display.
    *   **Texture Simulation:**  Map textures of on-screen content to the casing. E.g., a rough texture on an image results in a corresponding localized vibration pattern on the casing.
    *   **Contextual Feedback:** Provide haptic feedback for system events (e.g., notification alerts, button presses) *on the casing itself*, allowing for subtle and non-intrusive alerts.
*   **Ambient Light Integration:** Tie the intensity of haptic feedback to ambient light levels. Lower light = more subtle haptics. Higher light = more pronounced haptics.
*   **Gesture Recognition:** Implement gesture recognition on the casing itself.
    *   **Edge Squeeze:** Squeezing the sides of the device could trigger specific actions (e.g., volume control, launch an app).
    *   **Casing Tap:** Tapping the casing could act as a universal ‘back’ or ‘home’ button.
*   **Power Management:** Implement a dynamic power management system that adjusts the intensity and frequency of haptic feedback to conserve battery life. Option for user-defined haptic intensity levels.

**Pseudocode (Content Dragging):**

```
function onContentDrag(startDisplay, endDisplay, dragSpeed, contentSize):
  // Calculate drag path
  dragPath = calculatePath(startDisplay, endDisplay)

  // Calculate haptic intensity based on content size and drag speed
  hapticIntensity = contentSize * dragSpeed * 0.5 

  // Iterate along drag path, activating actuators
  for each point in dragPath:
    // Find closest actuators to point
    closestActuators = findClosestActuators(point)

    // Activate actuators with intensity proportional to hapticIntensity
    activateActuators(closestActuators, hapticIntensity)

    // Gradually decrease intensity as drag completes (damping effect)
    hapticIntensity = hapticIntensity * 0.95
```

**Further Considerations:**

*   Material Selection: Use materials for the casing that enhance haptic transmission.
*   Software API: Create a public API for developers to create custom haptic experiences.
*   Accessibility: Design haptic patterns that are accessible to users with visual impairments.