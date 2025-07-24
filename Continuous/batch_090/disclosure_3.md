# 11887580

## Dynamic Affect-Driven Vocal Performance

**Concept:** Expand the system's ability to tailor synthesized speech *beyond* quality/voice selection, by modeling nuanced vocal performance based on detected affect (emotion) within both the user input *and* the system’s generated response. This moves beyond simply *what* is said, to *how* it’s said.

**Specs:**

*   **Affect Detection Module:**
    *   Input: Natural Language Input (text or audio).
    *   Processing: Utilize a multimodal affect detection model (combining textual analysis with prosodic feature extraction from audio – pitch, tempo, intensity, spectral characteristics).  The model should output a continuous affect space representation (e.g., valence-arousal-dominance) for *both* the user’s input and the intended meaning of the system's output.
    *   Output: Affect Vectors – Two vectors, one representing user affect, and one representing intended system response affect.

*   **Vocal Performance Mapping:**
    *   Input:  System Response Affect Vector, Text of System Response.
    *   Processing: A learned mapping (neural network or rule-based system) translates the affect vector into a set of vocal performance parameters. These parameters control:
        *   *Pitch Range & Variation:*  Higher pitch and wider range for excitement/surprise.
        *   *Tempo & Rhythm:* Faster tempo for urgency, slower tempo for empathy.
        *   *Intensity & Dynamics:*  Loudness and variations in loudness to emphasize points or convey emotion.
        *   *Breathiness/Creakiness:*  Subtle variations in vocal quality to add realism and expressiveness.
        *   *Micro-pauses & Hesitations:* Strategic pauses to add nuance and avoid robotic delivery.
    *   Output: Vocal Performance Parameter Set

*   **Speech Synthesis Engine Adaptation:**
    *   Input: Text of System Response, Vocal Performance Parameter Set
    *   Processing:  The speech synthesis engine receives the text and parameter set.  The engine must be capable of dynamically adjusting its synthesis parameters *during* speech generation, not just at the beginning. This requires an API exposing fine-grained control over prosody and vocal quality. We may need to create a custom synthesis engine if existing options are too limited.
    *   Output: Synthesized Speech with dynamic vocal performance.

*   **User Affect Feedback Loop:**
    *   Input: User response (audio/text) after synthesized speech.
    *   Processing:  Re-analyze user response for affect. Compare detected affect with *expected* affect based on the system's intent.  If there is a significant discrepancy, adjust the affect mapping model to improve future performance.
    *   Output:  Updated affect mapping model.

**Pseudocode (Affect Mapping):**

```
function map_affect_to_parameters(affect_vector):
    // affect_vector = [valence, arousal, dominance]

    pitch_range = base_pitch_range + arousal * pitch_sensitivity
    tempo = base_tempo + valence * tempo_sensitivity
    intensity = base_intensity + arousal * intensity_sensitivity

    if valence < 0:  // Negative valence (sadness, anger)
        breathiness = base_breathiness + negative_valence_breathiness_factor
        pitch_variation = reduced_pitch_variation
    else:
        breathiness = base_breathiness

    // Dynamic adjustment based on dominance (power/submissiveness)
    if dominance > 0:
      pitch_variation = increased_pitch_variation
      intensity = increased_intensity

    return {
        "pitch_range": pitch_range,
        "tempo": tempo,
        "intensity": intensity,
        "breathiness": breathiness,
        "pitch_variation": pitch_variation
    }
```

**Novelty:**  While existing systems can *select* a voice based on user profile, this system actively *performs* the speech, imbuing it with nuanced emotional expression. This creates a more natural, engaging, and human-like interaction. The feedback loop ensures the system adapts to individual user responses, making the performance even more personalized.