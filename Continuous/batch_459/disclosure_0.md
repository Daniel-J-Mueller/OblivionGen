# 11016234

**Dynamic Light Diffusion & Haptic Feedback Integration**

**Concept:** Expand upon the light guide and touch sensing integration to create a dynamically diffusing light source *and* localized haptic feedback system. Instead of a uniform backlight, modulate light emission *through* the light guide to simulate textures, shapes, and even ‘depth’ visible through the display, combined with localized vibrations.

**Specs:**

*   **Light Guide Material:** Replace current light guide material with a multi-layered polymer incorporating micro-lens arrays *and* micro-fluidic channels. Channels will contain a refractive index-matching fluid.
*   **Micro-Lens Array:** Density: 500-1000 lenses/inch². Each lens will be individually addressable via micro-electromechanical systems (MEMS).
*   **Micro-Fluidic System:** Each channel will be connected to a micro-pump system allowing precise control of fluid distribution. This alters refractive index locally, bending light within the guide.
*   **Haptic Actuators:** Integrate an array of piezo-electric actuators *behind* the light guide, aligned with the micro-lens array. These will provide localized vibrations.
*   **Control System:** A dedicated processor handles both light modulation *and* haptic feedback. Input sources: touch sensor data, display content, application commands.
*   **Software API:** Provide a software API allowing developers to create custom light/haptic effects synchronized with display content.
*   **Display Integration:** Designed for seamless integration with existing electrophoretic displays, leveraging existing OCA layers for bonding.
*   **Power Requirements:** System to operate on 3.3V – 5V, minimizing power consumption.
*    **Calibration Routine:** Automated calibration routine to adjust fluid distribution and actuator strengths.

**Operation:**

1.  Touch sensor detects user input.
2.  Control system analyzes input and corresponding display content.
3.  Based on analysis, the control system:
    *   Activates specific lenses in the micro-lens array to modulate light diffusion – creating the illusion of textures or shapes *under* the display.
    *   Activates corresponding piezo-electric actuators to provide localized haptic feedback.
4.  The combination of modulated light and localized vibrations enhances the user experience, creating a more immersive and intuitive interface.

**Pseudocode (Simplified Light Modulation):**

```
function modulate_light(x, y, intensity, shape) {
  // x, y: Coordinates on the light guide
  // intensity: Brightness level (0-100)
  // shape: Type of light pattern (e.g., circle, square, texture)

  // Calculate lens array indices corresponding to (x, y)
  lens_x, lens_y = calculate_lens_indices(x, y)

  // Adjust lens activation based on shape and intensity
  for (i = lens_x - radius; i <= lens_x + radius; i++) {
    for (j = lens_y - radius; j <= lens_y + radius; j++) {
      if (is_within_shape(i, j, shape)) {
        activate_lens(i, j, intensity)
      } else {
        deactivate_lens(i, j)
      }
    }
  }
}

function activate_lens(x, y, intensity) {
  // Control the micro-lens to change how light is diffused
  // Adjust microfluidic channel to adjust refractive index
  // Apply voltage to lens to adjust light path
}
```