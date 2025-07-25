# D1017105

## Dynamic Projection Night Light - “Aura”

**Concept:** A night light that doesn't *emit* light, but *projects* ambient light patterns onto the surrounding environment. It leverages micro-projectors and dynamic content generation to create a soothing, evolving atmosphere.

**Hardware Specifications:**

*   **Housing:** Spherical, translucent acrylic shell (15cm diameter). Internal cavity for electronics. Diffused exterior for even light spread.
*   **Light Source:** Array of 5 micro-projectors (resolution: 640x480) mounted on a precision pan/tilt mechanism. Each projector uses a low-power laser diode array capable of projecting RGB colors.
*   **Processing Unit:** Raspberry Pi 4 Model B (or equivalent) with 4GB RAM.
*   **Sensors:**
    *   Ambient Light Sensor: Measures room brightness to adjust projection intensity.
    *   Microphone: Detects sound levels (optional: for reactive patterns).
    *   Temperature/Humidity sensor: Used for pattern selection/creation.
*   **Power:** USB-C powered (5V/2A).
*   **Connectivity:** WiFi (802.11 b/g/n) for updates and control.

**Software Specifications:**

1.  **Core Engine:**  Processing framework (e.g., openFrameworks, Processing) running on the Raspberry Pi. Responsible for managing projector outputs, sensor data, and content generation.
2.  **Content Library:** Pre-loaded library of dynamic light patterns – e.g., flowing aurora borealis, gently swaying underwater scenes, abstract geometric shapes. These patterns are defined as procedural animations.
3.  **Procedural Animation System:**  A system for creating and modifying light patterns algorithmically. Parameters include:
    *   `WaveformType`: (Sine, Square, Triangle, Sawtooth, Noise)
    *   `Frequency`: (Hz) – Speed of animation.
    *   `Amplitude`: (0-1) – Brightness/Intensity.
    *   `ColorPalette`: (Array of RGB values) – Colors used in the pattern.
    *   `Shape`: (Circle, Square, Triangle, Custom SVG path)
    *   `MotionType`: (Linear, Ease-In, Ease-Out, Bounce)
4.  **Environmental Reactivity:**
    *   **Ambient Light Adaptation:** Adjust projection brightness based on room light levels. If it is already bright, the system defaults to minimal projection or turns off.
    *   **Sound Reactive Mode (Optional):**  Use FFT analysis of audio input to modulate animation parameters (frequency, amplitude, color).
    *   **Temperature/Humidity Mode:** Selection of patterns based on temperature and humidity. A colder, drier room may trigger a 'frosty' pattern, while a warmer, humid room triggers a 'tropical' pattern.
5.  **User Interface:** Mobile app (iOS/Android) for:
    *   Pattern selection.
    *   Custom pattern creation (using simple parameter adjustments).
    *   Scheduling (set patterns to activate at specific times).
    *   Firmware updates.

**Operation:**

The “Aura” night light projects dynamic light patterns onto walls, ceilings, and other surfaces. The projected light appears to *emanate* from the surfaces, creating a calming and immersive ambiance. The device automatically adjusts brightness based on ambient light. Users can customize patterns and scheduling through the mobile app. The pan/tilt mechanism allows for subtle movement of the projected patterns.