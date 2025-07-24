# 10521181

## Dynamic Spectral Filtering Display Stack

**Concept:** Integrate microfluidic channels within the first adhesive layer (OCA) to dynamically adjust the spectral transmittance of the display stack. This allows for real-time color correction, blue light filtering, and potentially even rudimentary display capabilities *within* the adhesive layer itself.

**Specifications:**

*   **Adhesive Layer 1 (Dynamic Filter OCA):**
    *   Base Material: Acrylic-based OCA with embedded microfluidic channels (channel width: 50-100 micrometers, channel spacing: 200-500 micrometers).
    *   Microfluidic Network:  Layered network of interconnected channels spanning the entire display area.  Channels are sealed and chemically compatible with the OCA.
    *   Fluid:  A non-conductive, optically tunable fluid. This could be a solution of dichroic dyes, photochromic compounds, or even micro-particles with adjustable refractive indices.  Fluid viscosity must be compatible with microfluidic flow and prevent settling.
    *   Actuation:  Integrated micro-pumps and valves (piezoelectric preferred for speed & size) control fluid distribution within the channels.  These are addressable via a display controller. Pump rate: 0.1-1 microliter/second per channel. Valve resolution: 100+ addressable zones.
    *   Transmittance Range:  Tunable across the visible spectrum (390nm-700nm). Target: 0-100% transmittance at specific wavelengths.
    *   UV Filter: Optional inclusion of static UV absorbing particles to supplement dynamic filtering.
*   **Adhesive Layer 2 (Silicon OCA):** Standard silicon-based OCA for structural integrity.  Refractive index: 1.38-1.43.
*   **Adhesive Layer 3 (LOCA):** Standard LOCA for coupling to the display component. Refractive index: 1.35-1.45, thickness 145-185 micrometers.
*   **Control System:**
    *   Algorithm: Color calibration algorithm to match display output to target color space.  Blue light filtering profiles for eye comfort.  Dynamic adjustment based on ambient light conditions (using integrated light sensor).
    *   Interface: Communication with the display controller via I2C or SPI.  Power consumption: < 50mW.

**Pseudocode (Color Calibration):**

```
// Initialize: Read initial display color profile and ambient light level.
// Calibration Loop:
    // 1. Get current display output (RGB values).
    // 2. Measure actual displayed color using color sensor.
    // 3. Calculate color difference (Delta E).
    // 4. If Delta E > threshold:
        //  a. Determine required spectral adjustment (based on color difference).
        //  b. Send control signals to micro-pumps/valves to adjust fluid distribution in OCA.
        //  c. Wait for fluid to stabilize (50-100ms).
        //  d. Repeat color measurement.
    // 5. Repeat loop every frame (60Hz).
```

**Potential Benefits:**

*   Enhanced color accuracy and dynamic range.
*   Adaptive blue light filtering for improved eye health.
*   Potential for creating rudimentary display elements *within* the adhesive layer (e.g., displaying notifications).
*   Improved viewing experience in various lighting conditions.