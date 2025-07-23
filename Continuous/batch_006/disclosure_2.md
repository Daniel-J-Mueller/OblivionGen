# 9207450

## Pixel-Selective Backlighting with Micro-LED Array

**Concept:** Integrate a micro-LED array *behind* the electrowetting display, allowing for pixel-level backlighting control. This moves beyond simple on/off control and enables dynamic contrast enhancement, color gamut expansion (via additive color mixing), and the potential for creating a "pseudo-holographic" effect by selectively illuminating pixels at different intensities and timings. The existing "notch" in the electrode could serve as a routing channel for micro-LED control circuitry.

**Specs:**

*   **Micro-LED Array:**
    *   Density: Matched 1:1 with electrowetting pixel count. (Example: 1920x1080 array for a Full HD display)
    *   LED Size: <50µm.
    *   Material: GaN-based micro-LEDs for high efficiency and lifespan.
    *   Color: RGB micro-LEDs, individual control per pixel.
*   **Backplane/Control Circuitry:**
    *   TFT Integration: Leverage existing TFT backplane alongside dedicated micro-LED drivers. Each pixel (electrowetting + micro-LED) driven independently.
    *   Addressing: Row/Column matrix addressing for micro-LED control.
    *   Data Transmission: High-speed serial data interface (e.g., MIPI) for dynamic control.
    *   PWM Dimming: Pulse-Width Modulation (PWM) for precise brightness control of each micro-LED.
*   **Electrowetting Display Integration:**
    *   Transparent Electrode: Electrowetting electrode material must be highly transparent to allow light from the micro-LED array to pass through. ITO or similar.
    *   Optical Coupling: Implement an optical coupling layer between the micro-LED array and the electrowetting layer to maximize light transmission and minimize internal reflections.
    *   Notch Utilization: The notch described in the patent is utilized as a channel to route the micro-LED control circuitry (trace routing for power and data signals). This minimizes the need for additional layers and simplifies manufacturing. The organic dielectric material can be extended to cover the circuitry in the notch for protection.
*   **Software/Firmware:**
    *   Dynamic Contrast Algorithm: Implement an algorithm to dynamically adjust the micro-LED brightness based on the electrowetting pixel state. Black pixels are fully off, while bright pixels are boosted for increased contrast.
    *   Color Gamut Enhancement: Use the RGB micro-LEDs to expand the color gamut beyond the limitations of the electrowetting pigments.
    *   Pseudo-Holographic Mode: Develop algorithms to create a 3D-like effect by selectively illuminating pixels with varying intensity and timing. This could be used for simple notifications or interactive elements.
*   **Manufacturing:**
    *   Micro-LED Transfer: Utilize micro-LED transfer techniques (e.g., laser-induced forward transfer or pick-and-place) to accurately position the micro-LEDs onto the substrate.
    *   Wafer-Level Packaging: Utilize wafer-level packaging to encapsulate and protect the micro-LED array and control circuitry.

**Pseudocode (Dynamic Contrast Algorithm):**

```
For each pixel:
    electrowetting_state = read_electrowetting_state()
    brightness = calculate_brightness(electrowetting_state)
    set_microled_brightness(brightness)

Function calculate_brightness(electrowetting_state):
    If electrowetting_state == BLACK:
        Return 0
    Else If electrowetting_state == WHITE:
        Return MAX_BRIGHTNESS
    Else:
        Return MIN_BRIGHTNESS + (electrowetting_state * (MAX_BRIGHTNESS - MIN_BRIGHTNESS))
```