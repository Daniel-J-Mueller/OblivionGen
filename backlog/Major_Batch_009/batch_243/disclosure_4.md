# 8286885

## Dual-Display Haptic Overlay System

**Concept:** Expand the dual-display concept to incorporate localized haptic feedback, creating a system where the second display *feels* like a physical overlay on the first. This aims to create a more intuitive and tactile reading/interaction experience.

**Hardware Specifications:**

*   **First Display:** High-resolution E-Ink display (for content presentation – text, images, etc.). Target: 300 DPI, 10” diagonal.
*   **Second Display:** Flexible OLED display with integrated micro-actuator array. Target: 7” diagonal, 150 DPI. This display is positioned *directly* above the E-Ink display, with a minimal gap (approx. 2mm) filled with a transparent, flexible polymer.
*   **Micro-Actuator Array:** Piezoelectric actuators, spaced at 2mm intervals beneath the OLED display. Each actuator is individually controllable, capable of producing localized pressure variations (simulating texture, button presses, etc.).
*   **Haptic Driver:** Dedicated processor responsible for translating software commands into actuator control signals. Must support high-frequency ( >100Hz) actuator control for smooth haptic effects.
*   **Transparent Polymer Layer:** A flexible, optically clear polymer (e.g., PDMS) filling the gap between the displays. This layer serves as a pressure transmission medium and protects the OLED from direct contact.
*   **Housing:** Lightweight aluminum alloy, designed to minimize vibration and maximize rigidity.

**Software Specifications:**

*   **Haptic API:** Software library providing developers with functions to create and control haptic effects. Parameters include:
    *   `haptic_texture(x, y, texture_id, intensity)`: Creates a localized texture at coordinates (x, y) using a pre-defined texture ID and intensity level.
    *   `haptic_button(x, y, state)`: Simulates a button press at coordinates (x, y), with ‘state’ indicating pressed or released.
    *   `haptic_scroll(direction, speed)`: Creates a tactile scrolling sensation.
    *   `haptic_force_feedback(x, y, force_vector)`: Applies a localized force at coordinates (x, y).
*   **Content Mapping System:** Automatically analyzes content on the E-Ink display and generates corresponding haptic effects. For example:
    *   Maps page edges to a subtle tactile boundary.
    *   Highlights selectable elements (buttons, links) with a gentle pulsing sensation.
    *   Provides tactile feedback during scrolling.
*   **User Customization:** Allows users to adjust haptic intensity, texture profiles, and feedback types.

**Pseudocode (Content Mapping System):**

```pseudocode
function analyzeContent(eInkDisplayContent):
  visualElements = detectVisualElements(eInkDisplayContent)

  for each element in visualElements:
    if element.type == "button":
      generateHapticButtonEffect(element.x, element.y)
    else if element.type == "link":
      generateHapticHighlightEffect(element.x, element.y)
    else if element.type == "pageEdge":
      generateHapticBoundaryEffect(element.x, element.y)
```

**Novelty:** The combination of a secondary flexible display *directly* over the primary E-Ink display *with* localized, individually controllable haptic feedback creates a genuinely novel and immersive reading experience. The direct physical proximity of the displays, coupled with the haptic layer, blurs the line between digital and physical interaction.