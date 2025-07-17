# 9494790

**Dynamic Spectral Shifting Microfluidics**

**Concept:** Extend the reflective particle concept to create a dynamically tunable spectral filter within the electrowetting device. Instead of just reflecting light *back*, control *which wavelengths* are reflected.

**Specifications:**

1.  **Particle Composition:** Core-shell particles. Core: Strontium titanate (SrTiO3) - tunable refractive index with applied electric field. Shell: Silicon dioxide (SiO2) – for stability and biocompatibility. Introduce a doping agent within the SrTiO3 core, like iron or manganese, to enhance electric field sensitivity and spectral tunability. Particle size: 75-150nm.
2.  **Liquid Vehicle:**  Polar, high dielectric constant liquid (e.g., ethylene carbonate + propylene carbonate blend) with dissolved lithium perchlorate for increased ionic conductivity. Optimized viscosity for particle suspension and fast electrowetting response. 
3.  **Electrode Configuration:**  Interdigitated microelectrodes beneath the fluid layer.  Allows for creating localized electric fields and precise control of particle alignment and spectral tuning. Electrode material: Indium Tin Oxide (ITO) with a thin adhesion layer of titanium.
4.  **Device Architecture:**  Microfluidic chamber with two opposing glass substrates.  Bottom substrate: ITO electrodes. Top substrate: Transparent conductive oxide (TCO) layer for back-illumination or external light source coupling. Chamber height: 50-100um.
5.  **Control Algorithm:** 
    *   Input: Desired spectral transmission/reflection profile.
    *   Process: Algorithm calculates the required voltage distribution across the interdigitated electrodes to achieve the desired spectral profile. The algorithm incorporates a pre-characterized map of voltage-to-spectral shift for the specific particle and fluid combination.
    *   Output: Voltage waveform applied to the electrodes.
    *   Pseudocode:

```
function spectral_shift(desired_spectrum):
    // 1. Pre-characterization data: voltage_map[wavelength] = voltage
    // 2. Calculate error between desired_spectrum and current_spectrum
    error = desired_spectrum - current_spectrum
    // 3. For each wavelength band in error:
        // Calculate required voltage change: delta_voltage = voltage_map[wavelength] - current_voltage[wavelength]
        // Apply delta_voltage to corresponding electrode segment
    // 4. Update current_voltage with new voltage values
    // 5. Repeat steps 2-4 until error is minimized
    return voltage_waveform
```

6.  **Tunability Parameters:** 
    *   Wavelength range: 400-700nm (visible spectrum).
    *   Spectral resolution: 10nm bandwidth.
    *   Switching speed: < 10ms.

7.  **Applications:** 
    *   Dynamic color displays: Create high-contrast, full-color displays with tunable spectral characteristics.
    *   Adaptive optical filters:  Real-time spectral filtering for various sensing and imaging applications.
    *   Micro-spectrometers: Integrated micro-spectrometers for point-of-care diagnostics and environmental monitoring.