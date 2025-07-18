# 10627694

## Dynamic Micro-Lens Array for EPD/OLED Hybrid Displays

**Concept:** Integrate a dynamically adjustable micro-lens array *above* the color filter array of the hybrid EPD/OLED display. This array focuses or diffuses light from both the EPD and OLED segments, creating localized brightness and contrast enhancement, and potentially enabling parallax-based 3D effects without requiring complex glasses.

**Specs:**

*   **Micro-Lens Array Material:**  Electrowetting-based liquid lens material encapsulated in a transparent polymer matrix.  Polymer should exhibit high optical clarity and low hysteresis.
*   **Lens Density:**  100-200 lenses per square millimeter, matching or exceeding the pixel density of the underlying EPD/OLED array.
*   **Lens Control:** Each micro-lens is individually addressable via a transparent electrode layer patterned *beneath* the lens material.  Addressing scheme utilizes PWM (Pulse Width Modulation) to control the electrowetting voltage and therefore the lens focal length.
*   **Electrode Material:**  Indium Tin Oxide (ITO) or conductive polymer.  Must be highly transparent and have low sheet resistance.
*   **Layer Stack (Top to Bottom):**
    1.  Protective Overcoat (UV-curable acrylic)
    2.  Electrowetting Liquid (dielectric fluid)
    3.  Electrode Layer (ITO/Conductive Polymer)
    4.  Alignment Layer (to control liquid wetting)
    5.  Color Filter Array (existing)
    6.  Hybrid EPD/OLED Display
*   **Control System:**  A dedicated microcontroller with sufficient processing power to address each lens individually and implement dynamic lighting effects.  Input from a camera or user interface allows for interactive control.
*   **Addressing Scheme:** Each micro-lens is assigned a unique address. A 2D array of control lines delivers the electrowetting voltage to each lens. 
*   **Power Requirements:** Low voltage (e.g., 5V) for electrowetting control.
*   **Pseudocode for Dynamic Brightness Adjustment:**

```
// Initialize lens array and communication channels

function adjustBrightness(pixelX, pixelY, brightnessLevel) {
  // Calculate voltage based on brightness level (0-255)
  voltage = map(brightnessLevel, 0, 255, 0, maxVoltage);

  // Send voltage to corresponding lens address
  sendVoltage(pixelX, pixelY, voltage);
}

// Main Loop:
// For each pixel in display:
//    If pixel is OLED:
//        Adjust OLED brightness directly
//    If pixel is EPD:
//        Adjust lens focus to maximize/minimize perceived brightness.
//        (Experiment with diffusion for broader viewing angles).

// Function to dynamically adjust lens curvature to match EPD state
function adjustLensForEPD(pixelX, pixelY, ePDState) {
  if (ePDState == WHITE) {
    // Focus light for bright reflection
    setLensFocus(pixelX, pixelY, FOCUSED);
  } else if (ePDState == BLACK) {
    // Diffuse light to minimize reflection
    setLensFocus(pixelX, pixelY, DIFFUSED);
  }
}
```

**Innovation Notes:** The synergy of EPD and OLED is maintained, but enhanced with dynamic control over light. This could create visuals with incredible depth, contrast, and viewing angles. Beyond simple brightness control, the lens array allows for light steering, potentially implementing basic holographic effects. The control algorithm will be critical to balance the differing requirements of the EPD and OLED segments, and to minimize power consumption.