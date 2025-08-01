# 10521181

**Dynamic Spectral Filtering Display Stack**

**Concept:** A display stack incorporating microfluidic channels within the adhesive layers to allow dynamic adjustment of spectral filtering. This moves beyond static UV filtering to create a display that can adapt its color profile and reduce blue light emission on demand.

**Specs:**

*   **Adhesive Layers:** Replace the first (acrylic) and second (silicon) OCAs with microfluidic-integrated OCAs. Material: Optical silicone or acrylic with embedded microchannels (channel width: 50-100 micrometers, channel spacing: 200-500 micrometers).
*   **Microfluidic System:** Integrated micro-pumps and reservoirs to circulate a spectrally selective fluid (tunable dye or quantum dot solution) through the microchannels. Pump actuation: Piezoelectric or micro-electromechanical systems (MEMS).
*   **Fluid Composition:** Spectrally tunable fluid containing:
    *   Dye molecules with absorption peaks spanning the visible spectrum (e.g., cyan, magenta, yellow). Concentration control via microfluidic valves.
    *   Quantum dots with narrow emission spectra, allowing precise color mixing.
    *   UV absorbing compounds to enhance UV protection.
*   **Control System:**
    *   Software interface for user-defined color profiles (e.g., "reading mode," "gaming mode," "night mode").
    *   Ambient light sensor for automatic adjustment of spectral filtering based on environmental conditions.
    *   Display driver integration for dynamic control of spectral filtering in sync with displayed content.
*   **Stack Components:**
    *   Front Light Component: Light guide with diffuser.
    *   Touch Sensor: Capacitive or resistive touch sensor.
    *   Microfluidic OCA (First Layer): Between front light component and touch sensor.
    *   Microfluidic OCA (Second Layer): Between touch sensor and display component.
    *   Display Component: LCD, OLED, or E-paper display.
    *   LOCA: Liquid optically clear adhesive (LOCA) between display component and light guide; thickness: 145-185 micrometers.
*   **Fluid Circulation:** Integrated micro-pumps circulate the spectral filtering fluid through the microchannels. Fluid reservoirs are refilled periodically (e.g., every 6-12 months) via a user-accessible port.
*   **Refractive Index Matching:**  Careful selection of fluid refractive index to minimize light scattering and maintain image clarity.
*   **Power Consumption:** Low-power micro-pumps and control system. Target power consumption: < 50mW.

**Pseudocode (Control System):**

```
function adjust_spectrum(mode):
  if mode == "reading":
    set_fluid_concentration(blue_dye, 0.1)
    set_fluid_concentration(red_dye, 0.5)
    set_fluid_concentration(green_dye, 0.4)
  elif mode == "gaming":
    set_fluid_concentration(blue_dye, 0.7)
    set_fluid_concentration(red_dye, 0.3)
    set_fluid_concentration(green_dye, 0.3)
  elif mode == "night":
    set_fluid_concentration(blue_dye, 0.05)
    set_fluid_concentration(red_dye, 0.4)
    set_fluid_concentration(green_dye, 0.3)
  else:  // Default: full spectrum
    set_fluid_concentration(blue_dye, 1.0)
    set_fluid_concentration(red_dye, 1.0)
    set_fluid_concentration(green_dye, 1.0)

function auto_adjust_spectrum(ambient_light_level):
    if ambient_light_level < 10 lux:
        adjust_spectrum("night")
    elif ambient_light_level < 100 lux:
        adjust_spectrum("reading")
    else:
        adjust_spectrum("default")
```