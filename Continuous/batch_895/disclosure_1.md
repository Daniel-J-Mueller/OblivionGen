# 9684160

## Electrowetting Display with Dynamic Color Filtering via Microfluidic Dye Circulation

**Concept:** Enhance electrowetting displays beyond grayscale by integrating a microfluidic layer *above* the standard electrowetting cell, acting as a dynamic color filter. Instead of relying solely on voltage-controlled droplet shape for brightness modulation, we’ll use microchannels filled with primary colored dyes (Red, Green, Blue) and controlled by separate micro-pumps/valves. These dyes circulate based on the electrowetting cell state, essentially acting as a programmable color filter *over* each pixel.

**Specifications:**

*   **Layer Stack (from bottom to top):**
    1.  Substrate
    2.  Pixel Electrodes (as per the referenced patent)
    3.  Partition Walls (as per the referenced patent)
    4.  Fluorosurfactant Layer (as per the referenced patent – acts as hydrophobic barrier)
    5.  Microfluidic Layer:
        *   Material: PDMS (Polydimethylsiloxane) or similar biocompatible polymer.
        *   Channel Dimensions:  ~50µm width, ~30µm height (optimized for capillary action and pump efficiency).  Channel network patterned *above* each pixel.
        *   Channel Network: Each pixel will have a three-channel network (R, G, B).  Each channel connects to a central micro-pump/valve array located around the display perimeter.
        *   Dye Formulation: Water-based, highly concentrated primary color dyes (Red, Green, Blue).  Dye viscosity optimized for microfluidic flow.
*   **Micro-Pump/Valve Array:**
    *   Type:  Electroosmotic or piezoelectric micro-pumps/valves.
    *   Control:  Individual control for each RGB channel, per pixel.
    *   Flow Rate:  Precise control to modulate color intensity.
*   **Control Algorithm:**
    *   Input: Grayscale value for each pixel.
    *   Process:
        1.  Convert grayscale value to RGB components.
        2.  Calculate required flow rates for each RGB channel to achieve desired color intensity.
        3.  Send control signals to micro-pumps/valves.
*   **Pseudocode:**

```
For each pixel:
    grayscale_value = get_grayscale_value(pixel)
    red, green, blue = grayscale_to_rgb(grayscale_value)  // Conversion function

    red_flow_rate = map(red, 0, 1, 0, max_flow_rate)
    green_flow_rate = map(green, 0, 1, 0, max_flow_rate)
    blue_flow_rate = map(blue, 0, 1, 0, max_flow_rate)

    set_flow_rate(pixel.red_channel, red_flow_rate)
    set_flow_rate(pixel.green_channel, green_flow_rate)
    set_flow_rate(pixel.blue_channel, blue_flow_rate)
```

*   **Manufacturing Considerations:**
    *   Microfluidic layer fabricated using soft lithography techniques.
    *   Precise alignment of microfluidic channels with pixel electrodes.
    *   Integration of micro-pumps/valves onto a separate substrate and bonded to the display assembly.
*   **Potential Advantages:**
    *   Full-color display without requiring color filters, potentially increasing brightness and contrast.
    *   Dynamic color adjustment, allowing for a wider color gamut.
    *   Reduced power consumption compared to traditional backlit LCDs.