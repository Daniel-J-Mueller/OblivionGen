# 11501794

## Autonomous Affective Lighting System

**System Overview:** A mobile robotic platform equipped with multimodal sensors (camera, microphone, depth sensor) and an array of programmable, spatially distributed LED lights. The system aims to create an immersive, responsive lighting environment tailored to the detected emotional state of a user *and* the surrounding environment, going beyond simple sentiment detection to influence mood and create nuanced atmosphere.

**Core Innovation:**  Dynamic environmental sculpting with affectively-tuned illumination.  Rather than simply *detecting* sentiment, the system *responds* by altering the lighting to either amplify, soothe, or contrast the user's emotional state, considering the physical space.

**Hardware Specifications:**

*   **Robotic Platform:**  Differential drive mobile robot, minimum 60cm diameter, capable of navigating indoor environments.  Payload capacity: 5kg.
*   **Sensor Suite:**
    *   RGB-D Camera (Intel RealSense D435 or equivalent): For facial expression analysis, gesture recognition, and environmental mapping.
    *   Far-field Microphone Array (4-6 element):  For speech analysis and ambient sound detection.
    *   Inertial Measurement Unit (IMU): For robot localization and orientation.
*   **Lighting Array:**  200+ individually addressable RGBW LEDs (WS2812B or similar) distributed across a translucent, geometrically complex shell attached to the robotic platform. The shell should be roughly spherical with variable opacity sections.  Each LED has individual brightness and color control.
*   **Processing Unit:**  NVIDIA Jetson Xavier NX or equivalent embedded GPU for real-time sensor processing and lighting control.
*   **Power Supply:** High-capacity battery pack providing at least 4 hours of continuous operation.

**Software Architecture:**

1.  **Multimodal Data Acquisition:**
    *   Simultaneous acquisition of RGB-D images, audio data, and IMU data.
    *   Sensor synchronization to ensure data alignment.

2.  **Emotion & Environmental Analysis:**
    *   **Facial Expression Recognition:** Deep learning model (e.g., CNN) trained on a large dataset of facial expressions to classify emotions (happy, sad, angry, fearful, neutral, surprised, etc.).
    *   **Speech Sentiment Analysis:**  Natural Language Processing (NLP) model to analyze speech content and determine sentiment. Consideration of prosody (tone, pitch, rhythm) alongside lexical content.
    *   **Ambient Sound Analysis:** Identify dominant sound events (music, conversation, noise) to assess the environmental context.
    *   **Environmental Mapping:** Utilize RGB-D data to construct a 3D map of the surrounding environment, identifying surfaces, objects, and spatial layout.

3.  **Affective Lighting Control:**

    ```pseudocode
    function generateLightingPattern(emotion, environment, userPosition)
        // Define base lighting themes for each emotion
        themes = {
            "happy": { "color": "yellow", "intensity": 0.8, "pattern": "pulsating" },
            "sad": { "color": "blue", "intensity": 0.3, "pattern": "static" },
            "angry": { "color": "red", "intensity": 1.0, "pattern": "strobe" },
            // ... more themes
        }

        baseTheme = themes[emotion]

        // Environmental adaptation
        if environment.isDark():
            baseTheme.intensity = min(1.0, baseTheme.intensity * 1.5) // Increase intensity in dark rooms
        if environment.containsBrightColors():
            baseTheme.color = complementColor(baseTheme.color) // Shift color to contrast

        // User Position adaptation
        if userPosition.distanceToRobot < 1 meter:
            baseTheme.intensity = min(1.0, baseTheme.intensity * 0.5) // Subdued lighting for close proximity

        // Lighting Pattern Generation:  Assign colors and intensities to individual LEDs
        for led in LEDArray:
            led.color = baseTheme.color
            led.intensity = baseTheme.intensity * spatialFunction(led.position, userPosition)

        return LEDArray
    ```

4.  **Robot Navigation:**  Robot navigates the environment autonomously, positioning itself strategically to maximize the impact of the lighting and avoid obstructing the user's movement.  Utilizes SLAM (Simultaneous Localization and Mapping) for accurate localization and path planning.

**Novel Aspects:**

*   **Dynamic Environmental Sculpting:** The system doesn’t just change color; it shapes the light to create immersive environments.
*   **Multimodal Integration:** Combines facial expression, speech, ambient sound, and environmental mapping for a holistic understanding of the user's emotional state and surroundings.
*   **Proactive Affective Response:**  Rather than simply reacting to emotions, the system *influences* mood and atmosphere through strategic lighting design.  For example, a 'calming' response to detected anxiety.
*   **Spatial Lighting Control:**  Precisely controls individual LEDs to create nuanced and dynamic lighting patterns, adapting to the user's position and the environment’s layout.