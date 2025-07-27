# 9841595

## Microfluidic Lens Array with Electrowetting Control

**Concept:** Create a dynamically adjustable microfluidic lens array where each lens is formed by an electrowetting-controlled droplet sandwiched between transparent substrates. This allows for real-time focusing and beam steering at a microscale.

**Specs:**

*   **Substrate Material:** Fused Silica or BK7 glass (high optical clarity, thermal stability).
*   **Microchannel Dimensions:**
    *   Width: 50 – 200 μm (defines lens aperture).
    *   Length: 500 μm – 2 mm (defines focal length range).
    *   Height: 20 – 50 μm (defines droplet confinement).
*   **Electrode Configuration:**
    *   Indium Tin Oxide (ITO) electrodes patterned on the bottom substrate, forming an array corresponding to the microchannel array.
    *   Electrode pitch: 50 – 200 μm (matching microchannel width).
    *   Electrode shape: Circular or square, optimized for droplet confinement.
*   **Fluid System:**
    *   Droplet Fluid:  High refractive index oil (e.g., silicone oil) – refractive index > 1.4.
    *   Continuous Phase: Low refractive index fluid (e.g., Fluorinert FC-70) – refractive index < 1.3.
    *   Surfactant:  Fluorosurfactant to reduce surface tension and stabilize the droplet interface.
*   **Optical Stack (Based on Provided Patent):**
    *   **Bottom Substrate:** Fused Silica
    *   **Electrode Layer:** ITO (acts as reflective surface, Claim 2)
    *   **First Layer:** Silicon Nitride (Si3N4) - Optical thickness quarter-wave at 600nm (green light) (Based on Claim 1, optical thickness control) - Acts as reflective coating enhancement.
    *   **Second Layer:** Poly(vinylidene fluoride) (PVDF) (Claim 11) - Optimized dielectric properties for fast electrowetting response.
    *   **Microchannel Layer:** Etched into PVDF layer.
    *   **Top Substrate:** Fused Silica.
*   **Control System:**
    *   High-voltage power supply (0-100V) for applying voltage to electrodes.
    *   Microcontroller for sequencing voltage to individual electrodes and controlling lens array.
    *   Image sensor/camera for real-time feedback and calibration.

**Operation:**

1.  Microchannels are filled with the continuous phase fluid.
2.  Droplets of the high refractive index oil are injected into the microchannels, forming individual lenses.
3.  Applying voltage to individual electrodes changes the droplet shape due to electrowetting, altering the lens focal length.
4.  By sequentially controlling voltage to different electrodes, the lens array can be scanned or steered.

**Pseudocode for Control System:**

```
initialize electrodes array
initialize image sensor

loop:
    capture image from image sensor
    process image to identify target
    calculate required focal length changes for each lens
    for each lens in lens array:
        calculate voltage required for target focal length
        set voltage on corresponding electrode
    repeat
```

**Potential Applications:**

*   Miniature endoscopes.
*   Adaptive optics for microscopy.
*   Microfluidic beam steering for optical communication.
*   Dynamic focusing for lab-on-a-chip devices.