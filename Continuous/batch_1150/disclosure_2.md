# 9613624

## Dynamic Acoustic Event Prioritization for Streaming ASR

**Concept:** The existing patent focuses on reducing latency by pruning hypotheses. This builds on that by *prioritizing* acoustic events *before* full hypothesis generation, influencing the decoding graph construction itself. Instead of reacting to processing load, this proactively shapes it.

**Specification:**

**I. System Components:**

1.  **Front-End Acoustic Feature Extraction:** Standard feature extraction (MFCCs, etc.).
2.  **Acoustic Event Detector (AED):**  A lightweight neural network (e.g., a small CNN or RNN) trained to identify salient acoustic events within each frame (e.g., plosives, fricatives, vowel onsets, silence).  Output is a probability distribution over event classes.
3.  **Event Priority Assignor (EPA):**  Assigns a priority score to each frame based on the AED output.  Priority is a function of event probability *and* event class.  (e.g. plosives/fricatives receive higher priority than silence, even at lower probability).
4.  **Dynamic Graph Constructor (DGC):** Modifies the decoding graph construction based on frame priority. Higher priority frames trigger expansion of more graph nodes/arcs. Lower priority frames lead to a more constrained graph.
5.  **Standard ASR Decoder:** Performs decoding on the dynamically constructed graph.

**II. Algorithm (Pseudocode):**

```pseudocode
// Per Frame Processing
function process_frame(audio_frame):
    features = extract_features(audio_frame)
    event_probabilities = AED(features)
    frame_priority = EPA(event_probabilities)

    if frame_priority > threshold:
        // Expand graph nodes/arcs aggressively
        expand_graph(graph, features, aggressive_expansion_factor)
    else:
        // Constrain graph – fewer nodes/arcs
        expand_graph(graph, features, conservative_expansion_factor)

    return graph

//Initialization
graph = initialize_graph()

//Streaming Loop
while audio_stream:
    frame = get_next_frame(audio_stream)
    graph = process_frame(frame)
    decoded_output = ASR_Decoder(graph)
    output_results(decoded_output)
```

**III. Key Parameters & Configuration:**

*   `aggressive_expansion_factor`: Controls how much the graph is expanded for high-priority frames.
*   `conservative_expansion_factor`: Controls graph expansion for low-priority frames.
*   `AED_Training_Data`:  Dataset used to train the Acoustic Event Detector. Focus on data that captures nuances of speech articulation.
*   `Priority_Weighting`:  Weights assigned to different event classes within the EPA. (Tunable based on language/application).
*   `Threshold`: Priority score that determines the level of graph expansion.

**IV. Potential Benefits:**

*   **Reduced Latency:** By focusing decoding resources on salient acoustic events, the system can potentially achieve lower latency without sacrificing accuracy.
*   **Improved Robustness:** Prioritizing critical events can improve robustness to noise and challenging acoustic conditions.
*   **Adaptability:**  The priority weighting can be adapted to specific applications or acoustic environments.