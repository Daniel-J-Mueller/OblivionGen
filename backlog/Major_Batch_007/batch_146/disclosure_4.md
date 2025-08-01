# 10297211

## Dynamic Pixel-Level Backlighting with Micro-LED Array

**Concept:** Integrate a micro-LED array *behind* each pixel in the electrowetting display, controlled independently based on the photo-sensor reading *and* the electrowetting state. This allows for dynamic, pixel-level backlighting, boosting contrast and enabling novel visual effects.

**Specs:**

*   **Micro-LED Array:** Each pixel region will incorporate a micro-LED array (minimum 9 LEDs per pixel – 3x3 grid). LED pitch: < 50 microns. Emission spectrum: tunable white light with adjustable color temperature.
*   **Photo-Sensor Integration:** The existing photo-sensor remains, but its primary function shifts to *driving* the micro-LED array in addition to its existing role in TFT control.
*   **Control Circuit Enhancement:**  The existing control module is expanded to include a dedicated PWM (Pulse Width Modulation) controller for each micro-LED array. The PWM duty cycle is determined by a combined signal derived from:
    *   Photo-sensor voltage (representing ambient light/transmitted light)
    *   Electrowetting state (determined by TFT drive signal)
*   **Electrowetting/Backlight Correlation:** A lookup table (or learned model) defines the optimal backlight level (PWM duty cycle) for each electrowetting state.  This allows for predictive backlight adjustment. For example:
    *   Fully 'on' electrowetting state = minimal backlight (high contrast).
    *   Fully 'off' electrowetting state = maximum backlight (visibility in bright environments).
*   **Power Management:** Individual micro-LEDs are addressed and controlled to minimize power consumption. Dynamic power scaling based on ambient light levels.
*   **Display Stack-Up:**  The micro-LED array is positioned *behind* the electrowetting layer, physically separated by a transparent spacer layer. This ensures even illumination without interfering with electrowetting operation.
*   **Communication Protocol:**  SPI or I2C communication between the control module and the micro-LED driver ICs.

**Pseudocode (Control Module Update):**

```
// Inside main loop:

// Read photo-sensor voltage
photoSensorValue = readPhotoSensor();

// Read electrowetting state (TFT drive signal)
electrowettingState = readElectrowettingState();

// Lookup optimal backlight level
backlightLevel = lookupBacklightLevel(electrowettingState, photoSensorValue);

// Adjust backlight level (PWM duty cycle)
setBacklightLevel(backlightLevel);

// Apply TFT drive signal to control electrowetting
applyTFTDriveSignal();
```

**Novelty:** Current electrowetting displays rely on ambient or global backlighting. This design enables *pixel-level* dynamic backlighting, significantly improving contrast, visibility, and creating new visual possibilities (e.g., localized highlighting, dynamic shadows, improved HDR).  It moves beyond simply controlling light *through* the pixel to actively controlling light *from behind* the pixel, offering a far richer visual experience. It isn't just about making the display brighter or darker; it's about sculpting light at the pixel level.