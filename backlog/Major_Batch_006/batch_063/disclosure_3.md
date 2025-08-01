# 9684161

**Electrowetting Display with Dynamic Reflective Micro-Lenses**

**Concept:** Integrate micro-lens arrays *within* the reflective partition walls of an electrowetting display. These micro-lenses would not be static, but dynamically adjustable – their focal length and beam steering controlled by microfluidic channels also embedded within the partition walls. This allows for active light management *at the pixel level*, beyond simple reflection.

**Specifications:**

*   **Partition Wall Material:** Multi-layer construction. Base layer: Highly reflective dielectric material (Titanium Dioxide preferred). Intermediate Layer: Microfluidic channels etched into a transparent polymer. Top Layer: Micro-lens array fabricated from a refractive index tunable fluid (e.g., a solution of nanoparticles with controlled concentration).
*   **Micro-Lens Array:** Hexagonal close-packed arrangement. Lens diameter: 5-20 micrometers (scale with pixel density). Focal length: Dynamically adjustable from infinity (collimated light) to approximately the thickness of the display cell.
*   **Microfluidic Control:** Each micro-lens is individually addressable via integrated microfluidic channels. Control fluid: Electrically conductive fluid (same as used in electrowetting cells) allows for electrostatic control of fluid movement within the channels.
*   **Control Algorithm:**
    1.  **Ambient Light Sensing:** Integrate light sensors near each pixel.
    2.  **Dynamic Adjustment:** Based on ambient light level and image data:
        *   **High Ambient Light:** Adjust lenses to collimate reflected light *away* from the viewer, reducing glare and wash-out. Redirect light towards light-harvesting elements for energy recovery.
        *   **Low Ambient Light:** Focus reflected light *towards* the viewer, enhancing brightness and contrast.
        *   **Image-Based Control:** Dynamically shape reflected light to create localized contrast enhancement or directional illumination effects. For example, focus light on brighter areas of an image, or create a 'halo' effect around objects.
*   **Fabrication:**
    1.  Deposit reflective dielectric layer onto substrate.
    2.  Etch microfluidic channels using photolithography and reactive ion etching.
    3.  Deposit transparent polymer layer to encapsulate channels.
    4.  Micro-mold or inkjet-print refractive index tunable fluid into channel structures to form lenses.
    5.  Integrate with electrowetting cells and apply encapsulation layers.
*   **Power Requirements:** Minimal. Electrostatic control of microfluidic channels requires only low-voltage signals.



**Pseudocode (Control Algorithm):**

```
FOR EACH pixel:
    ambientLight = readAmbientLightSensor()
    imageBrightness = getImageBrightness()

    IF ambientLight > thresholdHigh:
        lensFocalLength = infinity  // Collimate light away
        redirectLightToEnergyHarvester()
    ELSE IF ambientLight < thresholdLow:
        lensFocalLength = optimalFocalLength // Focus light towards viewer
    ELSE:
        lensFocalLength = calculateFocalLength(imageBrightness) // Adjust based on image content

    setLensFocalLength(pixel, lensFocalLength)

```