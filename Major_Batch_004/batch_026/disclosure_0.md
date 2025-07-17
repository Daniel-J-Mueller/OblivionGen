# 10274804

**Modular, Multi-Layered EPD with Haptic Feedback**

**Concept:** Expand beyond simply curving the display around a frame. Create a truly modular electrophoretic display system composed of multiple, individually addressable layers, enabling dynamic 3D effects and integrated haptic feedback.

**Specs:**

*   **Layered Construction:** The display comprises at least three EPD layers:
    *   **Base Layer:** Standard EPD layer for primary image display. Flexible substrate.
    *   **Depth Layer:** A thinner EPD layer specifically for simulating depth and parallax. This layer uses a higher concentration of particles for stronger contrast in the depth simulation. Flexible substrate.
    *   **Haptic Layer:** A transparent, flexible layer containing microfluidic channels filled with a ferrofluid. This layer sits *above* the depth layer.
*   **Backplane:** A shared, multi-layer flexible backplane capable of addressing each EPD layer independently. Addressability down to individual pixels per layer.
*   **Microfluidic Control:** Each pixel in the haptic layer corresponds to a microfluidic chamber. By controlling the flow of ferrofluid within these chambers (using integrated micro-pumps and valves driven by the backplane), localized pressure variations create tactile sensations.
*   **Electrostatic Focusing:** Integrate electrostatic lenses above the haptic layer. These lenses can dynamically focus the visual information from the depth layer, enhancing the perception of 3D depth.
*   **Frame Integration:** Support frame designed with channels to accommodate and protect the layered structure and wiring. Frame material should be non-magnetic to avoid interfering with the haptic layer.
*   **Software Control:**  Software API for controlling each layer independently, creating dynamic 3D effects and haptic feedback patterns. This API must also facilitate the synchronization of visual and tactile information.
*   **Power Supply:** Low power, flexible power delivery system integrated into the frame or backplane.

**Pseudocode (Haptic Feedback Control):**

```
function updateHapticPixel(x, y, intensity, duration):
    // intensity: 0-100 (percentage of chamber filled with ferrofluid)
    // duration: milliseconds

    activateMicroPump(x, y)
    setPumpRate(rate = intensity * maxPumpRate) //scale pump rate to the target intensity
    delay(duration)
    deactivateMicroPump(x, y)

function createHapticWave(xStart, yStart, xEnd, yEnd, speed, amplitude):
    for each pixel along the wave path:
        calculate delay based on distance and speed
        call updateHapticPixel(pixelX, pixelY, amplitude, delay)
```

**Refinement Notes:**

*   The depth layer’s particle concentration and viewing angle can be optimized to maximize the perception of 3D depth.
*   Different ferrofluid formulations can be explored to tune the intensity and responsiveness of the haptic feedback.
*   The electrostatic lenses' focusing power and arrangement can be optimized to achieve the desired level of depth enhancement.
*   Potential for integrating force sensors into the haptic layer to create a truly interactive display experience.