# 10281713

## Dynamic Reflectance Control via Microfluidic Lens Array

**Concept:** Integrate a microfluidic lens array *above* the electrowetting pixel array to dynamically control light focusing *and* dispersion, enabling a wider range of apparent reflectance states beyond simple on/off switching. This expands the color palette and contrast achievable with an electrowetting display.

**Specs:**

*   **Microfluidic Layer:** A transparent polymer (e.g., PDMS) layer patterned with an array of micro-channels. Each micro-channel forms a micro-lens. The array is aligned 1:1 with the electrowetting pixel array below.
*   **Working Fluid:** A clear, high-refractive index fluid fills the micro-channels. Viscosity is tuned for fast response times.
*   **Actuation:** Each micro-channel incorporates a micro-heater (thin-film resistor) or a micro-pump (piezoelectric).
*   **Control Scheme:**  A dedicated driver circuit controls the temperature (or pumping rate) of each micro-channel.
    *   *Heating:* Increasing temperature locally alters the refractive index of the fluid, changing the focal length of the micro-lens. This dynamically focuses or diverges light from the corresponding electrowetting pixel.
    *   *Pumping:* Micro-pumping allows for precise control over the fluid volume within each micro-lens, altering its shape and focal length.
*   **Pixel Interconnection:** The TFT controlling each electrowetting pixel is *also* connected to the driver circuit for its corresponding micro-lens. This enables coordinated control.
*   **Layer Stack (Bottom to Top):**
    1.  Support Plate
    2.  Electrowetting Pixel Array (as per provided patent)
    3.  Transparent Insulating Layer (to isolate pixel array from microfluidic layer)
    4.  Microfluidic Layer with integrated micro-lenses and actuation elements.
    5.  Transparent protective layer.

**Pseudocode (Control Algorithm – per Pixel):**

```
function updatePixel(electrowettingState, desiredReflectance) {
  // electrowettingState: 0 (off) or 1 (on)
  // desiredReflectance: 0.0 to 1.0

  if (electrowettingState == 0) {
    // Pixel is "off" – use micro-lens to control dispersion
    microLensPower = calculateMicroLensPower(desiredReflectance, dispersionMode);
    setMicroLensPower(microLensPower);
  } else {
    // Pixel is "on" – use micro-lens to refine focus/reflectance
    microLensPower = calculateMicroLensPower(desiredReflectance, focusMode);
    setMicroLensPower(microLensPower);
  }
}

function calculateMicroLensPower(desiredReflectance, mode) {
  // Implement algorithm to map desired reflectance to micro-lens power
  // Algorithm considers:
  //  - Initial reflectance from electrowetting state
  //  - Micro-lens characteristics (focal length, aperture)
  //  - Mode (dispersion or focus)
  // Return calculated micro-lens power
}

function setMicroLensPower(power) {
  // Send control signal to micro-heater/pump to achieve desired power
  // Adjust heater current/pump rate based on power value
}
```

**Expected Outcome:**  By dynamically adjusting the micro-lens array, the system can:

*   Increase contrast by focusing light from "on" pixels and diverging it from "off" pixels.
*   Simulate a wider range of colors by combining the on/off state of the electrowetting pixel with the focusing/diverging effect of the micro-lens.
*   Create 3D-like effects by selectively focusing light from certain pixels.