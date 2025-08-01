# 9588336

## Microfluidic Lens Arrays for Dynamic Focal Control

**Concept:** Integrate microfluidic channels *within* the spacer layers of the electrowetting display to create dynamically adjustable micro-lenses. This allows for localized focal control *within* the display itself, moving beyond simple pixel illumination and enabling advanced optical effects.

**Specs:**

*   **Spacer Layer Material:**  Transparent, flexible polymer (e.g., PDMS, fluoropolymer) compatible with electrowetting fluids and microfabrication techniques. The spacer layer *must* be capable of withstanding capillary forces and fluid pressure.
*   **Microchannel Dimensions:**  Channel width: 5-50μm. Channel height (variable): 1-10μm. Channel length: Dependent on pixel size (approx. 100-500μm). These dimensions are critical for controlling the focal length and resolution of the micro-lens.
*   **Fluid:**  Clear, high refractive index fluid (e.g., silicone oil, fluorocarbon oil) compatible with both the spacer material and the electrowetting fluid. The fluid must be chemically inert and exhibit low evaporation.
*   **Actuation Method:** Electrowetting-driven micro-pumps integrated *adjacent* to the micro-lens channels. These pumps will precisely control the flow of fluid *into* and *out* of the channels, changing the channel volume and thus the lens curvature (focal length). A separate electrode layer will be required for the pumps within the spacer layer, connected to the display's control circuitry.
*   **Channel Layout:** Channels arranged in a grid corresponding to the pixel arrangement. Each pixel or group of pixels will have one or more micro-lens channels associated with it.
*   **Spacer Layer Construction:**
    1.  Fabricate a base layer for the spacer grid (similar to the existing patent process).
    2.  Using microfabrication techniques (e.g., photolithography, etching, soft lithography), create the microchannel network *within* this base layer.
    3.  Bond a top layer to seal the microchannels, ensuring leak-proof operation.
    4.  Integrate the micro-pumps and control electrodes.
*   **Control Algorithm:** Software control to adjust the voltage applied to the micro-pumps, thereby controlling the fluid flow and lens curvature.  The algorithm should allow for:
    *   Individual pixel/lens control.
    *   Pre-programmed focal patterns (e.g., depth of field effects).
    *   Real-time focal adjustment based on sensor input (e.g., object tracking).

**Pseudocode (Control Algorithm – Simplified):**

```
FUNCTION adjust_lens(pixel_x, pixel_y, focal_length):
  // focal_length is desired focal length (in arbitrary units)
  // pixel_x, pixel_y identify the pixel/lens to control

  target_volume = calculate_volume(focal_length) // Volume needed for target focal length
  current_volume = read_volume(pixel_x, pixel_y) // Current fluid volume in the lens

  volume_difference = target_volume - current_volume

  IF volume_difference > 0:
    activate_pump(pixel_x, pixel_y, "fill", volume_difference) // Fill the lens with fluid
  ELSE IF volume_difference < 0:
    activate_pump(pixel_x, pixel_y, "drain", ABS(volume_difference)) // Drain fluid from the lens
  ENDIF

ENDFUNCTION
```

**Potential Applications:**

*   Dynamic depth of field control for enhanced 3D displays.
*   Microscopic imaging directly integrated into the display.
*   Adaptive optics to correct for display distortions.
*   Holographic display elements.