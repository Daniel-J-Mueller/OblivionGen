# 11941517

## Adaptive Temporal Granularity for Entity Representations

**Concept:** The current patent focuses on encoding a time-ordered sequence of events. This design expands on that by introducing *adaptive* temporal granularity during the encoding process. Instead of treating each event in the sequence as a uniform time step, the system dynamically adjusts the granularity based on event density and/or the magnitude of change indicated by the event itself.

**Specification:**

**I. Granularity Detection Module:**

*   **Input:** Time-ordered sequence of events, each with a timestamp and event data.
*   **Process:**
    1.  **Time Difference Calculation:** Calculate the time difference between consecutive events.
    2.  **Event Change Magnitude:**  Calculate a change magnitude score for each event. This can be a composite score considering feature differences in the event data. For example, a purchase event with a high dollar amount has a high change magnitude. An event simply viewing a product has a lower score.
    3.  **Granularity Score:** Combine time difference and change magnitude to create a 'Granularity Score' for each event.  High change magnitude and/or large time differences indicate a need for finer granularity.
    4.  **Thresholding:** Apply thresholds to the Granularity Score to determine the granularity level for each event. Levels could be: 'Fine', 'Medium', 'Coarse'.

**II. Adaptive Encoding Layer:**

*   **Input:** Time-ordered sequence of events, granularity levels assigned by the Granularity Detection Module.
*   **Process:**
    1.  **Event Expansion/Contraction:** Based on the assigned granularity level:
        *   **Fine:** The event remains as a single time step.
        *   **Medium:** The event is duplicated in the sequence, effectively extending its temporal presence.
        *   **Coarse:** The event is skipped, essentially compressing time.
    2.  **Sequence Padding/Truncation:**  The dynamically adjusted sequence might need padding or truncation to maintain a fixed length for the encoder.
    3.  **Encoding:** The adjusted sequence is fed into the encoder (e.g., RNN, LSTM).

**III. Decoder Integration:**

*   The decoder remains largely unchanged, receiving the fixed-size representation from the encoder.
*   The decoder's performance can be evaluated with varying granularity settings to optimize the system.

**Pseudocode (Granularity Detection Module):**

```python
def calculate_granularity(event_sequence):
  granularity_scores = []
  for i in range(len(event_sequence)):
    time_diff = 0
    if i > 0:
      time_diff = event_sequence[i]['timestamp'] - event_sequence[i-1]['timestamp']

    change_magnitude = calculate_event_change(event_sequence[i]) # Custom function

    granularity_score = (0.7 * change_magnitude) + (0.3 * (1/time_diff)) #Weightings adjustable

    granularity_scores.append(granularity_score)

  granularity_levels = []
  for score in granularity_scores:
    if score > 0.8:
      granularity_levels.append("Fine")
    elif score > 0.4:
      granularity_levels.append("Medium")
    else:
      granularity_levels.append("Coarse")

  return granularity_levels
```

**Potential Benefits:**

*   **Improved Representation Accuracy:**  Capturing significant events with finer granularity while compressing less important time periods.
*   **Reduced Computational Cost:**  By compressing time, the sequence length can be reduced, leading to faster processing.
*   **Enhanced Model Robustness:**  Adaptability to varying event densities and data quality.