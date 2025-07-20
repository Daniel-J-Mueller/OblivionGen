# 11521599

## Dynamic Temporal Context for Wakeword Detection

**Concept:** Expand beyond fixed-window analysis of audio to incorporate a dynamically adjusting temporal context, leveraging a recurrent neural network (RNN) to model the ‘expectancy’ of the wakeword based on preceding audio. This moves beyond simply *detecting* the wakeword to *predicting* its potential arrival, enabling faster, more robust detection, and reduced false positives.

**Specs:**

*   **Input:** Raw audio waveform, sampled at 16kHz. Feature extraction will use a Mel-Frequency Cepstral Coefficient (MFCC) pipeline, with 40 coefficients, delta and delta-delta features.

*   **Core Model:** A stacked bidirectional Long Short-Term Memory (LSTM) network.
    *   LSTM Layer 1: 256 units.
    *   LSTM Layer 2: 128 units.
    *   Output Layer: Fully connected layer with sigmoid activation. Outputs a single value between 0 and 1, representing the ‘wakeword likelihood’ at that time step.

*   **Temporal Context Window:**  Variable length, dynamically adjusted based on a ‘context score’. 
    *   The initial context window is 2 seconds.
    *   The context score is calculated as the moving average of the wakeword likelihood output over the last 5 seconds.
    *   If the context score exceeds a threshold (e.g., 0.2), the context window *expands* by 0.5 seconds (up to a maximum of 5 seconds). This indicates a heightened probability of the wakeword occurring.
    *   If the context score falls below a lower threshold (e.g., 0.05), the context window *contracts* by 0.5 seconds.

*   **Sliding Window Analysis:** The LSTM processes the audio in short, overlapping windows (e.g., 25ms windows with 10ms overlap).

*   **Final Detection Layer:** A separate feedforward neural network processes the LSTM’s output, along with features from a convolutional neural network (CNN) analyzing the current audio window. This combines temporal context with short-term spectral analysis.
    *   CNN: 3 convolutional layers with increasing filter sizes (3, 5, 7) and ReLU activation.
    *   Fully connected layer with sigmoid activation for final wakeword detection.

*   **Training Data:** A large dataset of diverse speech samples including positive examples of the wakeword and a wide range of negative examples (normal speech, background noise).
*   **Loss Function:** Binary Cross-Entropy.

**Pseudocode:**

```
function detect_wakeword(audio_stream):
  context_window_size = 2.0  # seconds
  context_score = 0.0
  lstm_output_history = []

  for window in sliding_window(audio_stream, window_size=0.025, overlap=0.010):
    # Feature extraction (MFCCs)
    features = extract_mfccs(window)

    # LSTM Processing
    lstm_output = lstm_model.predict(features)
    lstm_output_history.append(lstm_output)

    # Context Score Calculation
    context_score = moving_average(lstm_output_history, window=5)

    # Dynamic Context Window Adjustment
    if context_score > 0.2:
        context_window_size += 0.5
        context_window_size = min(context_window_size, 5.0)
    elif context_score < 0.05:
        context_window_size -= 0.5
        context_window_size = max(context_window_size, 2.0)

    # CNN Feature Extraction
    cnn_features = extract_cnn_features(window)

    # Combined Feature Processing
    combined_features = concatenate(lstm_output, cnn_features)

    # Final Detection
    detection_score = detection_model.predict(combined_features)

    if detection_score > threshold:
        return True, timestamp  # Wakeword detected

    return False, timestamp
```

**Novelty:** This moves beyond a static temporal window to a dynamically adapting one, incorporating ‘expectancy’ based on preceding audio. This should allow for more robust detection in noisy environments and reduce false positives by predicting the likelihood of the wakeword’s arrival.