# 11567504

## Dynamic Environmental Storytelling via Robotic Proxemics

**Concept:** Expand the robot’s ‘wait location’ determination to create subtle, dynamic environmental storytelling. The robot doesn’t just *wait* – it *performs* a passive narrative based on detected user presence, detected emotional state (via audio/visual cues), and pre-programmed environmental ‘themes’.

**Specs:**

**1. Data Acquisition & Analysis Module:**

*   **Sensors:** Existing camera, microphone, plus:
    *   Low-resolution thermal sensor (detects approximate body heat signatures – for presence, not identification)
    *   Vibration sensor (detects footsteps/nearby movement even without visual confirmation)
*   **Processing:**
    *   Real-time audio analysis – detects voice tone, volume, and potentially emotional keywords.
    *   Computer Vision – basic facial expression recognition (happiness, sadness, anger, neutral). Focus on broad categories, not individual identity.
    *   Proximity Detection – integrates data from thermal, vibration, and vision to establish “user bubble” radius around the robot.

**2. Environmental Theme Database:**

*   Pre-defined thematic profiles. Examples:
    *   **“Calm”:** Slow, deliberate movements. Subtle ambient light adjustments (if robot has controllable lights).  Plays low-volume, soothing sounds (nature sounds, ambient music).  Favors wait locations near plants/natural elements.
    *   **“Playful”:** Slight rocking/bobbing movement while waiting.  Projects simple geometric patterns on nearby surfaces (if projector available).  Plays upbeat, whimsical sounds.
    *   **“Reflective”:**  Robot orients itself to face a window/artwork.  Minimal movement.  Plays melancholic music or ambient soundscapes.
    *   **“Alert”**: Rapidly scans the environment and will emit a red glow.
*   Themes are categorized by emotional impact (positive, negative, neutral) and complexity (simple, moderate, complex).
*   Database is expandable and customizable via a user interface.

**3.  Wait Location Selection & Performance Algorithm:**

```pseudocode
function selectWaitLocationAndPerform(occupancyMap, costMaps, userProximity, userEmotion):
  // 1. Initial Candidate Location Selection (as per original patent)
  candidateLocations = determineCandidateLocations(occupancyMap, costMaps)

  // 2. Theme Selection
  theme = selectTheme(userEmotion, userProximity, currentEnvironmentTheme)

  // 3. Location Scoring (modified to include theme suitability)
  for each location in candidateLocations:
    score = calculateScore(location, costMaps) + calculateThemeSuitability(location, theme)

  // 4. Select Best Location
  bestLocation = selectLocationWithHighestScore(candidateLocations)

  // 5. Perform Theme at Selected Location
  performTheme(bestLocation, theme)

function performTheme(location, theme):
  if theme == "Calm":
    // Slow, deliberate movements
    // Subtle light adjustments
    // Play soothing sounds
  else if theme == "Playful":
    // Slight rocking/bobbing
    // Project patterns
    // Play upbeat sounds
  else if theme == "Reflective":
    // Face window/artwork
    // Minimal movement
    // Play melancholic music
  // ... other themes
```

**4.  Robot Hardware Modifications:**

*   Low-power projector (optional - for “Playful” theme)
*   Controllable RGB LED array (for subtle ambient lighting effects)
*   Small, high-quality speaker
*   Improved vibration dampening (to allow for subtle movements without being disruptive)

**Novelty:**  This expands the robot’s function from *passive waiting* to *active environmental contribution*. It’s about subtly influencing the user’s experience through carefully curated robotic ‘performance’ in the environment.  It’s not about overt interaction, but rather a quiet, ambient storytelling that enhances the overall atmosphere.