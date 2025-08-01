# 8964276

## Dynamic Pixel Aperture Control via Microfluidic Lenses

**Concept:** Integrate microfluidic lenses directly *within* each pixel of the electro-wetting display to dynamically control the aperture and focal length of light emitted/reflected. This expands beyond simple on/off pixel control to create a pseudo-volumetric display effect and significantly improve contrast and viewing angles.

**Specs:**

*   **Pixel Structure:** Each pixel comprises the standard electro-wetting components (electrodes, fluid, pixel wall) *plus* a microfluidic chamber integrated above the fluid layer.
*   **Microfluidic Chamber:** A sealed chamber approximately 50-100µm in diameter, fabricated from a transparent polymer (e.g., PDMS or a similar biocompatible material) directly bonded to the second base substrate.
*   **Lens Fluid:** The chamber is filled with a clear, electrically controllable fluid with a high refractive index. This fluid's surface tension can be adjusted via a third electrode layer embedded within the second base substrate *beneath* the chamber.
*   **Electrode Layer:** This third electrode is patterned to allow independent control of the surface tension within each microfluidic chamber.
*   **Control Algorithm:**  Pseudocode:

    ```
    FOR each pixel IN display:
        READ desired brightness level (0-100)
        READ desired focal distance
        CALCULATE voltage to apply to pixel electrode (based on brightness)
        CALCULATE voltage to apply to microfluidic lens electrode (based on brightness & focal distance)
        APPLY voltages to both electrodes
    END FOR
    ```
*   **Operation:**
    *   **Brightness Control:** The standard electro-wetting mechanism controls pixel illumination.
    *   **Aperture Control:** By adjusting the voltage applied to the microfluidic lens electrode, the surface tension of the fluid within the chamber changes, altering its shape. This effectively adjusts the aperture of the lens.  Higher voltage = smaller aperture (sharper image, reduced brightness). Lower voltage = larger aperture (brighter image, softer focus).
    *   **Focal Length Control:**  Precise control of the microfluidic lens shape can also dynamically adjust the focal length, allowing for a limited degree of depth control.
*   **Materials:**
    *   Transparent conductive oxides (TCOs) for electrodes.
    *   PDMS or similar for microfluidic chambers.
    *   High refractive index fluid compatible with electro-wetting fluids.
*   **Fabrication:**
    *   Standard microfabrication techniques (photolithography, etching) to create electrode patterns.
    *   Soft lithography to create PDMS microfluidic chambers.
    *   Bonding of PDMS chambers to the second base substrate.
    *   Fluid filling via microchannels.

**Potential Benefits:**

*   Improved contrast ratios.
*   Wider viewing angles.
*   Pseudo-3D effect through dynamic focal length control.
*   Potential for creating novel display effects.