# 8950682

## Dual-Display Haptic Feedback System – “SensePage”

**Concept:** Expand the dual-display functionality beyond visual input to incorporate localized haptic feedback, creating a more immersive and intuitive reading/interaction experience.

**Specifications:**

*   **Display Configuration:** Retain the core concept of a primary, low-power E-Paper display paired with a secondary, higher refresh rate display (OLED/TFT). The secondary display will serve *primarily* as a haptic feedback array.
*   **Haptic Array:** The secondary display will integrate an array of micro-actuators (piezoelectric or similar) beneath the surface. Each actuator will be individually addressable. Resolution target: 60-80 actuators per square inch.
*   **Content Mapping:** Software will analyze text or graphics displayed on the primary E-Paper display. Based on content type (e.g., button, image edge, text character), corresponding haptic patterns will be generated and applied to the secondary display *directly adjacent* to the relevant area on the primary display.
*   **Haptic Library:** A pre-built library of haptic “textures” and “sensations” (e.g., click, ripple, texture, resistance) will be included. Developers will have API access to customize and create new sensations.
*   **Dynamic Friction Simulation:** Utilize the haptic array to simulate the feeling of “friction” when scrolling text or navigating lists. The resistance felt by the user's finger will correlate to the content being interacted with (e.g., more friction for a defined list item, less for free-flowing text).
*   **Contextual Haptics:**
    *   **Buttons/UI Elements:** When a virtual button on the E-Paper display is pressed, a localized "click" sensation will be delivered through the corresponding area on the secondary display.
    *   **Image Exploration:** As a user traces an image on the E-Paper display with their finger, the secondary display will generate subtle texture changes based on the image content (e.g., rough texture for stone, smooth for water).
    *   **Text Highlighting:** When text is highlighted, a localized “pulse” or “vibration” sensation will be delivered to the corresponding area on the secondary display.
*   **Input Integration:** The secondary display will also function as a touch-sensitive input surface. Combined with haptic feedback, this will provide a highly intuitive and precise input method.
*   **Power Management:** Implement a dynamic power saving mode. When the user is not actively interacting with the device, the haptic array will be deactivated.

**Pseudocode (Haptic Pattern Generation):**

```
function generateHapticPattern(contentData, location) {
  if (contentData.type == "button") {
    return createClickPattern(location);
  } else if (contentData.type == "image") {
    return createImageTexturePattern(contentData.textureMap, location);
  } else if (contentData.type == "text") {
    if (contentData.highlighted) {
      return createPulsePattern(location);
    } else {
      return createSmoothPattern(location); // Minimal haptic feedback
    }
  } else {
    return createSmoothPattern(location); // Default – no haptic feedback
  }
}

function createClickPattern(location) {
  // Generate a localized, short-duration vibration pattern
  // Adjust intensity and duration based on user preferences
  return {
    location: location,
    intensity: 70,
    duration: 50ms
  };
}

// Implement other pattern creation functions as needed (e.g., createImageTexturePattern, createPulsePattern)
```

**Materials:**

*   Primary Display: E-Paper (bistable LCD, electrophoretic, etc.)
*   Secondary Display: High-resolution OLED or TFT with integrated micro-actuator array.
*   Housing: Lightweight, durable material (e.g., aluminum alloy, polycarbonate).
*   Protective Layer: Scratch-resistant, anti-glare coating for both displays.