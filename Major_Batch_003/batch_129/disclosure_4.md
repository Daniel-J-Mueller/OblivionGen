# 12045568

## Dynamic Frame Resolution via Temporal Anchoring

**Concept:** Extend the span-based frame representation to incorporate a temporal dimension, allowing the system to resolve ambiguities and refine task execution based on the *order* in which information is presented within the user input. This addresses scenarios where the same keywords can signify different actions depending on context and timing.

**Specs:**

*   **Module:** Temporal Resolver
*   **Input:**
    *   Span-based frame representation (intents, slots, span endpoints)
    *   Raw input tokens with timestamps (if speech) or sequential order (if text)
    *   Encoder Feature Vector (from the provided patent)
*   **Process:**
    1.  **Anchor Point Identification:** Identify key “anchor” tokens within the input that signal temporal relationships (e.g., "first," "next," "before," "after," date/time expressions).
    2.  **Temporal Graph Construction:**  Build a graph where nodes are input tokens, and edges represent temporal relationships inferred from anchor points and sequential order.  Edge weights represent the strength of the temporal connection.
    3.  **Frame Propagation:** Propagate information from the span-based frame representation *through* the temporal graph.  This means that intent and slot values are "scored" based on their proximity to anchor points and the strength of the temporal connections.  Tokens closer to recent temporal anchors receive higher scores.
    4.  **Dynamic Span Refinement:** The original span endpoints (first and second index) are *adjusted* based on the scored tokens. The system prioritizes tokens with higher temporal scores when determining the final span representing the relevant information.
    5.  **Task Execution with Temporal Context:**  The refined span-based frame representation, now enriched with temporal information, is passed to the task execution module.

**Pseudocode:**

```
function refine_span(frame, tokens, timestamps):
  temporal_graph = build_temporal_graph(tokens, timestamps)
  scored_tokens = propagate_frame_through_graph(frame, temporal_graph)

  refined_span_start = find_highest_scored_token_start(scored_tokens)
  refined_span_end = find_highest_scored_token_end(scored_tokens)

  refined_frame = update_frame_span(frame, refined_span_start, refined_span_end)
  return refined_frame
```

**Hardware/Software Considerations:**

*   Requires a graph database or efficient graph data structure for managing the temporal graph.
*   The "propagation" step could leverage graph neural networks to learn complex temporal dependencies.
*   A dedicated module to identify and interpret temporal anchor points.
*   Integration with the existing encoder and length module.  The encoder’s output can be fed into the temporal graph construction process.