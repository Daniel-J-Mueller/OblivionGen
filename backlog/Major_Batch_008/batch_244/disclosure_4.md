# 10241377

## Dynamic Haptic & Thermal E-Paper Display

**Concept:** Expand the self-healing electrophoretic display concept to incorporate localized haptic feedback *and* temperature control directly within the display stack, creating a more immersive and interactive user experience. This goes beyond simple touch input; the display can *feel* like different textures or simulate temperature changes.

**Specs:**

*   **Display Stack Modification:** Integrate a layer of microfluidic channels *beneath* the electrophoretic layer, within the flexible TFT backplane. These channels will be filled with a thermally conductive and electrically tunable fluid (ET fluid – a dielectric fluid whose viscosity changes with applied voltage).

*   **Haptic Actuation:** Micro-pumps (MEMS based) will precisely control the flow of ET fluid within individual microfluidic chambers (effectively tiny “bubbles”) beneath the EPD. Increasing fluid pressure creates a localized bulge, mimicking texture. Rapid, localized changes in pressure generate haptic pulses. A grid of these chambers forms the haptic layer.

*   **Thermal Control:**  The same ET fluid will act as a heat transfer medium.  Micro-heaters (thin-film resistors) will be integrated alongside the microfluidic channels.  Precise control of the micro-heater’s power, coupled with fluid flow modulation, allows localized temperature changes on the display surface.  

*   **Power & Control:**  Additional conductive traces within the flexible TFT backplane will connect to the micro-pumps and micro-heaters. A dedicated control IC will manage the fluid flow, heater activation, and synchronization with the EPD content.  The control IC will expose an API for external applications to define haptic and thermal effects.

*   **Self-Healing Integration:** The existing self-healing properties of the cover lens and protective layers are crucial. The microfluidic channels *must* be constructed from a self-healing polymer or encapsulated in a protective self-healing material to prevent leaks and maintain functionality during stress or damage.

*   **Materials:**
    *   Flexible TFT backplane: Polyimide.
    *   Microfluidic Channels: Self-healing polymer (e.g., based on disulfide bonds).
    *   ET Fluid: Dielectric fluid with high thermal conductivity and tunable viscosity.
    *   Micro-heaters: Thin-film platinum resistors.
    *   Micro-pumps: MEMS-based piezoelectric pumps.

**Pseudocode (Control IC API):**

```
// Function to set haptic feedback for a given area
function setHapticEffect(x, y, intensity, duration, pattern) {
  // x, y: Coordinates of the area
  // intensity: Strength of the haptic feedback (0-100)
  // duration: Duration of the effect in milliseconds
  // pattern:  Predefined or custom haptic pattern (e.g., "pulse", "vibration", "texture")

  // Calculate pump activation parameters based on intensity, duration, and pattern
  pump_params = calculate_pump_parameters(intensity, duration, pattern)

  // Activate micro-pumps for the specified area based on pump_params
  activate_micro_pumps(x, y, pump_params)
}

// Function to set temperature for a given area
function setTemperature(x, y, temperature, duration) {
  // x, y: Coordinates of the area
  // temperature: Desired temperature in Celsius
  // duration: Duration of the effect in seconds

  // Calculate heater power based on temperature and duration
  heater_power = calculate_heater_power(temperature, duration)

  // Activate micro-heaters for the specified area based on heater_power
  activate_micro_heaters(x, y, heater_power)
}

// Function to synchronize haptic/thermal effects with EPD content
function synchronizeEffect(epd_frame, effect_data) {
  // epd_frame: Data for the current EPD frame
  // effect_data: Data describing the desired haptic/thermal effects
  // (e.g., effect type, area, intensity, duration)

  // Analyze epd_frame and effect_data to determine appropriate haptic/thermal effects
  // for each area of the display

  // Generate control signals for micro-pumps and micro-heaters
  // to create the desired effects in synchronization with the EPD content
}
```

**Potential Applications:**

*   Realistic simulations (e.g., feeling textures in a virtual shopping experience)
*   Enhanced gaming experiences (e.g., feeling the impact of a virtual object)
*   Accessibility features (e.g., providing tactile feedback for visually impaired users)
*   Medical applications (e.g., providing tactile feedback during remote surgery)