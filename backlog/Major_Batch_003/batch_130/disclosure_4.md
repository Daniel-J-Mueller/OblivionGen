# 12078896

## Dynamic Metasurface Pixel Array

**Concept:** Extend the amplitude/phase modulation pixel concept to create a dynamically reconfigurable metasurface. Instead of simply modulating light *through* the pixel, leverage the pixel itself *as* the metasurface element. This enables far greater control over wavefront shaping, beam steering, and polarization control.

**Specs:**

*   **Pixel Structure:** Each pixel comprises two liquid crystal cells as described in the reference, *plus* a micro-fabricated, electrically controllable diffraction grating. The grating is positioned *after* the two liquid crystal cells in the light path.
*   **Grating Material:** Thin-film lithium niobate (LiNbO3) or similar electro-optic material.
*   **Grating Control:** Individual micro-electrodes beneath each grating element. Allows for precise control of the grating period and amplitude.
*   **LC Cell Configuration:** Twisted nematic (TN) for amplitude control, vertically aligned (VA) for phase control. Polarizers optimized for high contrast and efficient modulation.
*   **Ground Plane:** Transparent ITO, shared between LC cells and grating.
*   **Addressing Scheme:** Each pixel is addressed with three control signals:
    1.  Voltage to TN cell (amplitude).
    2.  Voltage to VA cell (phase).
    3.  Voltage array to grating electrodes (diffraction pattern).
*   **Resolution:** Target pixel pitch of 5um – 10um.
*   **Refresh Rate:** > 1 kHz.
*   **Operating Wavelength:** 633nm - 850nm. (extendable with material choices.)

**Pseudocode (Pixel Control Loop):**

```
FOR each pixel IN pixel_array:
    target_amplitude = desired_amplitude[pixel.x, pixel.y]
    target_phase = desired_phase[pixel.x, pixel.y]
    target_diffraction_pattern = desired_pattern[pixel.x, pixel.y]

    // Calculate TN voltage
    tn_voltage = sqrt(target_amplitude)  // Or a calibrated transfer function

    // Apply VA voltage for target phase
    va_voltage = target_phase  // Or a calibrated transfer function

    // Calculate grating electrode voltages
    grating_voltages = calculate_grating_voltages(target_diffraction_pattern)

    // Apply voltages to pixel components
    apply_voltage(pixel.tn_cell, tn_voltage)
    apply_voltage(pixel.va_cell, va_voltage)
    apply_voltage(pixel.grating_electrodes, grating_voltages)
END FOR
```

**Functionality:**

*   **Wavefront Shaping:** Combine amplitude/phase modulation with grating control to sculpt complex wavefronts.
*   **Beam Steering:** Dynamically adjust the grating to steer the beam without mechanical movement.
*   **Holographic Projection:** Reconstruct 3D images by modulating the wavefront.
*   **Polarization Control:** Utilize the grating to tailor the polarization state of the light.
*   **Multi-view Displays:** Create multiple views of a scene from a single display.

**Materials:**

*   Liquid Crystals: High birefringence, fast switching speed.
*   Substrates: Glass, fused silica, or flexible polymers.
*   Electrodes: ITO, transparent conductive oxides.
*   Grating Material: Lithium Niobate, Gallium Arsenide, Silicon Nitride.

**Considerations:**

*   Heat dissipation due to switching.
*   Crosstalk between adjacent pixels.
*   Calibration of the system.
*   Fabrication complexity.
*   Drive Electronics.