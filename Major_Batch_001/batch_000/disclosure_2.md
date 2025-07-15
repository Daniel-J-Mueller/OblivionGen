# 10001637

## Dynamic Hydrophilicity Control via Nanoparticle Embedding

**Concept:** Integrate light-sensitive nanoparticles within the photoresist layer during fabrication. These nanoparticles would allow for *post-fabrication* dynamic control of surface hydrophilicity, enabling advanced display functionalities beyond simple electrowetting.

**Specs:**

*   **Nanoparticle Material:** Tungsten Disulfide (WS2) nanoparticles – chosen for their photo-responsiveness and established use in photodetectors. Alternatively, consider molybdenum disulfide (MoS2). Particle size: 5-10nm.
*   **Photoresist Composition:** SU-8, with WS2 nanoparticles uniformly dispersed at a concentration of 0.1-1% by weight.  Dispersant:  A non-ionic surfactant to prevent aggregation (e.g., Triton X-100).
*   **Fabrication Steps (Modified from Patent):**
    1.  Form pixel electrodes on substrate.
    2.  Prepare nanoparticle-embedded SU-8 photoresist solution.
    3.  Spin-coat photoresist onto pixel electrodes.
    4.  Apply light through mask (as in patent).  Mask design is crucial (see below).
    5.  Apply oxygen plasma treatment *before* development, as in patent. This initially sets the baseline hydrophilicity.
    6.  Develop photoresist to form water-repellent layer and partitioning walls.
    7.  **Novel Step: Post-Fabrication Illumination Control:** Integrate micro-LEDs *beneath* each pixel. These LEDs will emit light at a wavelength that selectively alters the surface charge of the WS2 nanoparticles, modulating the hydrophilicity of the corresponding partition wall surface.  Wavelength: 450-500nm (blue light).
*   **Mask Design Enhancement:**  The mask will consist of three distinct layers:
    *   Opaque: Defines partition wall boundaries.
    *   Translucent (Gray Scale): Controls the density of nanoparticle embedding within the partition walls. Higher density = greater potential for hydrophilicity modulation.
    *   Transparent: Corresponds to pad areas.
*   **Control System:**  A microcontroller will manage the micro-LEDs, allowing precise control over the hydrophilicity of each partition wall.  This will enable:
    *   **Dynamic Color Mixing:** By selectively altering the hydrophilicity of adjacent pixels, the effective 'color' of the display can be changed without requiring physical color filters.
    *   **Advanced Display Modes:**  Creation of novel display modes such as grayscale, variable transparency, or even 'holographic' effects.
    *   **Adaptive Contrast Control:**  Real-time adjustment of contrast based on ambient lighting conditions.

**Pseudocode (Control System):**

```
// Initialize LED array and pixel map
LED_array = initialize_LED_array()
pixel_map = initialize_pixel_map()

// Main Loop
while (true) {
    // Read display data (e.g., image frame)
    frame_data = read_frame_data()

    // For each pixel
    for (i = 0; i < pixel_count; i++) {
        // Calculate desired hydrophilicity level (based on frame_data)
        hydrophilicity_level = calculate_hydrophilicity(frame_data[i])

        // Map hydrophilicity level to LED intensity
        led_intensity = map_hydrophilicity_to_intensity(hydrophilicity_level)

        // Set LED intensity for corresponding pixel
        set_led_intensity(LED_array[i], led_intensity)
    }

    // Wait for next frame
    wait(frame_duration)
}
```

**Materials:**

*   SU-8 Photoresist
*   Tungsten Disulfide (WS2) Nanoparticles (5-10nm)
*   Non-ionic Surfactant (e.g., Triton X-100)
*   Micro-LED array
*   Transparent substrate (e.g., glass or PET)
*   Microcontroller and associated circuitry.