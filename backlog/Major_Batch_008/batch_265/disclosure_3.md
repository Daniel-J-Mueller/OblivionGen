# 9723035

## Real-Time Affective State Projection & Shared Sensory Input

**Concept:** Expanding beyond simple attendance & location, this system aims to project affective states (emotional responses) of meeting attendees onto a shared virtual space, augmenting it with personalized sensory input for enhanced remote collaboration.

**Specs:**

**I. Core System – Affective State Mapping**

1.  **Multi-Modal Sensor Fusion:** Utilize data streams from attendee mobile devices *beyond* location:
    *   **Camera:** Facial expression analysis (micro-expressions, gaze tracking).
    *   **Microphone:** Voice tone analysis (pitch, energy, pauses).
    *   **Accelerometer/Gyroscope:**  Detecting subtle body language cues (fidgeting, posture changes – requiring calibration per user).
    *   **Heart Rate/Skin Conductance (Optional - requires wearable integration):** For more granular emotional response data.
2.  **Affective State Engine:**  Employ a machine learning model (trained on diverse datasets) to correlate sensor data with probable emotional states (e.g., engaged, confused, frustrated, bored, excited). Output a confidence score for each state.
3.  **Virtual Avatar Representation:**  Each attendee is represented by a customizable avatar in a shared virtual meeting space.  Avatar features (color, shape, animation) are dynamically adjusted based on the detected affective state. For example:
    *   **Engagement:** Brighter colors, active animation (subtle nodding).
    *   **Confusion:**  Color desaturation, head tilt, questioning animation.
    *   **Frustration:**  Color shifts to red/orange, tense posture, quick glances.
4.  **Privacy Controls:**  Users can choose the level of affective state sharing:
    *   **Off:** No affective data is shared.
    *   **Simplified:** Only broad categories (Positive/Negative/Neutral) are shared.
    *   **Detailed:** Full affective state data is shared.
5.  **Calibration Phase:** Initial user calibration for baseline readings and personalized model training.
    *   Prompt users to express different emotions while sensor data is collected.

**II. Shared Sensory Augmentation**

1.  **Localized Haptic Feedback:** Integration with wearable haptic devices.  
    *   When an attendee expresses excitement, nearby avatars (in the virtual space) experience a subtle vibration.
    *   When an attendee expresses confusion, a gentle “prompt” vibration is sent to avatars positioned as facilitators or “experts”.
2.  **Directional Audio:**  Spatial audio system.
    *   When an attendee speaks, their voice is localized to their avatar’s position in the virtual space.
    *   Emotional inflection of voice is exaggerated *slightly* to highlight emotional cues.
3.  **Virtual “Ambient” Scents (Future Iteration):**  Integration with scent diffusion technology (if viable).
    *   Associate certain emotional states with specific scents (e.g., lavender for calm, citrus for energy).  Subtle scent diffusion triggered by attendee emotional states.
4.  **Personalized Virtual Backgrounds:** Based on detected emotion.
    *   Engaged - dynamic and bright
    *   Stressed - calming imagery

**III. Pseudocode - Affective State Projection**

```
// For each attendee:

function processSensorData(cameraData, microphoneData, accelerometerData, heartRateData) {
  // Feature extraction from each data stream
  facialFeatures = extractFacialFeatures(cameraData)
  voiceFeatures = extractVoiceFeatures(microphoneData)
  motionFeatures = extractMotionFeatures(accelerometerData)
  hrFeatures = extractHeartRateFeatures(heartRateData)

  // Combine features into a single vector
  combinedFeatures = [facialFeatures, voiceFeatures, motionFeatures, hrFeatures]

  // Predict affective state using trained ML model
  predictedState = mlModel.predict(combinedFeatures)

  return predictedState
}

function updateAvatar(attendeeID, predictedState) {
  // Adjust avatar color, shape, animation based on predictedState
  avatar = getAvatar(attendeeID)
  avatar.color = mapEmotionToColor(predictedState)
  avatar.animation = mapEmotionToAnimation(predictedState)
  // Apply haptic feedback, directional audio adjustments
  applyHapticFeedback(predictedState)
  adjustAudio(predictedState)
}

// Main Loop:
for each attendee in meeting {
  sensorData = getSensorData(attendee)
  emotionalState = processSensorData(sensorData)
  updateAvatar(attendee.ID, emotionalState)
}
```

**Novelty:** This system moves beyond simply knowing *who* is at a meeting to understanding *how* they are feeling and actively augmenting the remote collaboration experience with those cues, creating a more empathetic and engaging virtual environment. It focuses on *shared* experience beyond the visual.