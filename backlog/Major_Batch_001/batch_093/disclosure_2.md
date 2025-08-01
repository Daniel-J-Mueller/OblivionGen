# 10073211

## Dynamic Border Light Control for Reflective Displays

**Concept:** Expand upon the multilayer structure around the display border to create a dynamically controllable light emission zone, capable of displaying simple animations or providing contextual feedback to the user.

**Specs:**

*   **Layer Stack (Beyond Existing Patent):**
    *   Reflective Display
    *   Light Guide (as per patent)
    *   First OCA Layer
    *   Black Ink Layer (opacity adjustable – see control system)
    *   White Ink Layer (diffuse reflector)
    *   Micro-LED Array (integrated within white ink layer, high density, individually addressable)
    *   Diffuser Layer (thin film, to spread Micro-LED light evenly)
    *   Second OCA Layer
    *   Protective Outer Layer (clear, durable)

*   **Micro-LED Specifications:**
    *   Pixel Pitch: < 50µm
    *   Brightness: Adjustable, 0.1 - 1000 nits
    *   Color: RGB, full color spectrum
    *   Control: Addressable via serial communication (SPI or I2C)
    *   Power: Low power consumption, individual pixel control for efficiency

*   **Opacity Control System:**
    *   Electrically controllable ink: Utilize a material that changes opacity based on applied voltage. Integrate this into the black ink layer.
    *   Segmentation: Divide the black ink layer into small, independently controllable segments for fine-grained control of light leakage.
    *   Control Interface: Software API for adjusting opacity levels and creating animations.

*   **Software Implementation:**
    *   Driver: Dedicated driver to manage the Micro-LED array and the opacity control system.
    *   API: Simple API for developers to create custom lighting effects and animations.
    *   Preset Modes: Include pre-defined modes like:
        *   Notification Indicator: Flashing color for notifications.
        *   Charging Indicator: Smooth gradient for battery status.
        *   Ambient Lighting: Color matching the display content.

*   **Physical Dimensions:** Border width adjustable from 1mm – 5mm. Total thickness should not exceed 1mm.

**Pseudocode (Animation Control):**

```
function set_border_animation(animation_name):
    if animation_name == "notification":
        loop:
            set_border_color(red)
            delay(200ms)
            set_border_color(off)
            delay(800ms)

    elif animation_name == "charging":
        battery_level = get_battery_level()
        color = map_battery_level_to_color(battery_level)
        set_border_color(color)
    elif animation_name == "custom":
        // Load animation data from file or stream
        // Iterate through frames
        // For each frame:
        //   Set individual LED colors
    else:
        set_border_color(off)
```

**Rationale:**

This system moves beyond simply masking light leakage. It *transforms* the border into a dynamic display element. The micro-LED array allows for complex animations and visual feedback, while the opacity control system enables subtle effects and seamless integration with the display content. This creates a more immersive and informative user experience.