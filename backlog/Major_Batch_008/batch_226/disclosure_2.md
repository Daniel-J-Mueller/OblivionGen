# 9052501

## Electrowetting Spatial Light Modulator with Dynamic Masking

**Concept:** Expand beyond simple display states to create a spatially addressable light modulator using an array of individually controlled electrowetting cells. Incorporate a dynamically generated “mask” layer *within* the electrowetting cell itself, altering light transmission independently of the fluid configuration.

**Specs:**

*   **Cell Structure:** Each pixel comprises a multi-layered electrowetting cell.
    *   Layer 1: Transparent substrate (e.g., ITO coated glass).
    *   Layer 2: Electrowetting fluid (immiscible pair – oil/water example).
    *   Layer 3:  A microfluidic layer containing a secondary, highly scattering fluid (e.g., a suspension of nano-particles). This layer is contained within microchannels patterned *above* the primary electrowetting fluid.  The microchannels form a grid or array.
    *   Layer 4:  An array of individually addressable micro-heaters (thin film resistors) positioned *beneath* the scattering fluid microchannels. These heaters locally alter the viscosity of the scattering fluid.
    *   Layer 5: Transparent top substrate.
*   **Addressing Scheme:**
    *   Primary Electrowetting Control: Standard electrode arrangement to control the oil/water interface.
    *   Secondary Scattering Control:  Individual micro-heater control.  Increased temperature reduces scattering fluid viscosity, allowing it to redistribute and *reduce* light scattering in that pixel location.  Decreased temperature increases viscosity, maximizing scattering.
*   **Operation:**
    1.  Electrowetting Voltage: Applied to set the primary oil/water configuration (on/off, grayscale).
    2.  Scattering Control:  Independently controlled micro-heaters modulate the scattering fluid.
        *   High Temp/Low Viscosity:  Minimal scattering, allowing light transmission.
        *   Low Temp/High Viscosity:  Maximum scattering, blocking light transmission.
*   **Pseudocode (Pixel Control):**

```
function setPixel(x, y, electrowettingLevel, scatteringLevel) {
  // electrowettingLevel: 0-1 (grayscale)
  // scatteringLevel: 0-1 (0=transparent, 1=opaque)

  setElectrowettingVoltage(x, y, electrowettingLevel);
  setMicroheaterPower(x, y, scatteringLevel); // Controls viscosity/scattering
}

function displayImage(imageBuffer) {
  for each pixel (x, y) in imageBuffer {
    setPixel(x, y, imageBuffer[x,y].electrowetting, imageBuffer[x,y].scattering);
  }
}
```

*   **Materials:**
    *   ITO coated glass substrates.
    *   Electrowetting fluids (low voltage, high dielectric constant).
    *   Microfluidic polymers (PDMS, PMMA).
    *   Nano-particle suspension (TiO2, SiO2) in a carrier fluid.
    *   Thin film resistor materials (NiCr, ITO).

*   **Applications:**
    *   High-contrast displays.
    *   Spatial light modulators for beam steering/holography.
    *   Microscopes with dynamically adjustable contrast.
    *   Adaptive optics.