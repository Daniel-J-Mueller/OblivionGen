# 9529189

## Electrowetting Display - Dynamic Reflector Array

**Concept:** Enhance luminance and viewing angle by implementing a dynamically adjustable reflector array *behind* the electrowetting layer. This goes beyond static reflectors on partition walls by creating a micro-lens array that can shift and focus reflected light based on viewing angle and active pixel state.

**Specifications:**

*   **Layer Stack (added to existing electrowetting display):**
    *   Electrowetting Layer (as per patent 9529189)
    *   Transparent Substrate
    *   Micro-Electromechanical System (MEMS) Mirror Array
    *   Backlight/Ambient Light Source
*   **MEMS Mirror Array:**
    *   Each mirror corresponds to a subpixel or small group of subpixels.
    *   Mirrors are individually addressable via a control matrix (similar to an LCD panel).
    *   Mirrors have a limited range of tilt and pan (e.g., +/- 15 degrees).
    *   Mirror surface: Highly reflective dielectric multilayer coating (optimized for visible spectrum).
    *   Actuation: Electrostatic actuation preferred (low power, fast response). Alternative: Piezoelectric actuation.
*   **Control System:**
    *   Image processing unit analyzes the active pixel data (electrowetting state of each pixel).
    *   Based on the pixel data, the control system calculates the optimal tilt/pan angle for each mirror.
    *   Algorithm prioritizes directing reflected light towards the viewer's estimated position.
    *   Algorithm dynamically adjusts mirror angles to compensate for parallax and viewing angle changes.
    *   Calibration routine to account for mirror imperfections and variations.
*   **Light Management:**
    *   Backlight: Uniform, high-brightness LED array.
    *   Light guide plate to distribute light evenly across the display area.
    *   Diffuser layer to minimize hotspots and glare.
    *   Polarizer to reduce reflections and improve contrast.

**Pseudocode (Mirror Control Algorithm):**

```
// Input: Pixel Data (brightness, color), Viewer Position (x, y, z)
// Output: Mirror Angle (theta, phi) for each mirror

function calculateMirrorAngle(pixelBrightness, pixelColor, viewerX, viewerY, viewerZ):
    // Calculate ideal light direction based on viewer position and pixel location
    idealDirection = normalize(viewerX - pixelX, viewerY - pixelY, viewerZ - pixelZ)

    // Calculate reflection vector
    reflectionVector = reflect(idealDirection, surfaceNormal)

    // Adjust reflection vector based on pixel brightness and color
    if pixelBrightness > threshold:
        // Increase reflection angle to boost luminance
        reflectionAngle += adjustmentFactor
    if pixelColor == red:
        // Adjust reflection angle to enhance red saturation
        reflectionAngle += redAdjustmentFactor

    // Apply limits to reflection angle
    if reflectionAngle > maxAngle:
        reflectionAngle = maxAngle
    if reflectionAngle < minAngle:
        reflectionAngle = minAngle

    return reflectionAngle
```

**Further Considerations:**

*   Power consumption of the MEMS array.
*   Manufacturing complexity of the MEMS mirrors and control circuitry.
*   Durability and reliability of the MEMS mirrors.
*   Potential for cross-talk between adjacent mirrors.
*   Cost optimization of the overall system.