# 9449563

## Dynamic Sub-Pixel Color Mixing via Microfluidic Channel Networks

**Specification:**

**I. Core Concept:** Replace static, fixed-size sub-pixels in the electrowetting display with dynamically configurable microfluidic channels containing multiple immiscible fluids of differing primary colors (Red, Green, Blue, and potentially others like Yellow, Cyan, Magenta). Control the *ratio* of each color fluid within each ‘pixel’ space by applying localized voltages to control the fluid flow through the microchannels. This enables precise color mixing *within* each pixel, exceeding the color gamut of traditional sub-pixel arrangements.

**II. Hardware Components:**

*   **Electrowetting Layer:** Standard electrowetting layer as described in the provided patent, modified to incorporate microfluidic channel inlets/outlets.
*   **Microfluidic Network:** A dense network of microchannels (width < 5µm, depth < 10µm) etched into a transparent substrate (e.g., glass or polymer). Each pixel will contain a localized network, branching and converging to create a mixing chamber.
*   **Fluid Reservoirs:** Small, individually addressable reservoirs for each color fluid, connected to the microfluidic network.
*   **Micro-Valves:** Integrated micro-valves (electrowetting or other microfluidic actuation methods) controlling the flow of each fluid within the network. Addressable by row/column matrix similar to LCD addressing.
*   **Transparent Electrodes:** ITO or similar transparent electrodes patterned to address both electrowetting and micro-valve actuation.
*   **Backplane Control:** A backplane with individual row/column drivers for both electrowetting and micro-valve control.

**III. Operation:**

1.  **Initial State:** All pixels initially contain a uniform mixture of all color fluids, appearing white or grey.
2.  **Color Generation:** To generate a specific color:
    *   Apply voltages to open/close specific micro-valves, controlling the relative flow rates of each color fluid into/out of the pixel’s mixing chamber.
    *   Simultaneously apply electrowetting voltages to manipulate the fluid interfaces within the mixing chamber, maximizing mixing efficiency and achieving the desired color ratio.
    *   The electrowetting layer will still act to control light transmission, essentially 'shuttering' the mixed color.
3.  **Gray-Scale Control:** Adjust the electrowetting voltage to control the amount of light transmitted through the pixel, providing gray-scale control for each color.
4.  **Dynamic Range & Gamut:** By precisely controlling the fluid ratios and light transmission, a significantly wider color gamut and dynamic range can be achieved compared to traditional sub-pixel arrangements.

**IV. Control Algorithm (Pseudocode):**

```
// Input: Desired RGB color values (R, G, B) for pixel
// Output: Voltage levels for electrowetting and micro-valve control

function calculate_voltages(R, G, B):
    // Calculate fluid ratios based on RGB values
    red_ratio = R / (R + G + B)
    green_ratio = G / (R + G + B)
    blue_ratio = B / (R + G + B)

    // Calculate micro-valve voltages based on fluid ratios
    valve_red = map(red_ratio, 0, 1, 0, valve_max_voltage)
    valve_green = map(green_ratio, 0, 1, 0, valve_max_voltage)
    valve_blue = map(blue_ratio, 0, 1, 0, valve_max_voltage)

    // Calculate electrowetting voltage based on overall brightness
    brightness = (R + G + B) / 255.0  // Scale to 0-1
    ew_voltage = map(brightness, 0, 1, 0, ew_max_voltage)

    return ew_voltage, valve_red, valve_green, valve_blue
```

**V. Enhancements:**

*   **Multi-Layer Networks:** Stacking multiple microfluidic layers to increase color mixing complexity and precision.
*   **Dynamic Channel Reconfiguration:** Integrate microfluidic channels with shape-memory alloys or other actuators to dynamically reconfigure the channel network, enabling advanced color effects.
*   **Self-Healing Channels:** Utilize self-healing polymers to repair minor channel damage and ensure long-term reliability.