# 11120361

## Adaptive Granularity Time Series Decomposition & Ensemble

**Concept:** Extend the routing/ensembling framework to dynamically decompose time series into variable-length segments *before* training, and train specialized algorithms on these segments. This allows the system to learn patterns at multiple granularities *simultaneously*, adapting to differing levels of complexity within a single time series.

**Specifications:**

1.  **Decomposition Module:**
    *   Input: Raw time series data (first time series from item descriptor).
    *   Function: Implements a rolling-window change point detection algorithm (e.g., Bayesian Online Change Point Detection – BOCPD).
    *   Parameters:
        *   `sensitivity`: Controls the responsiveness of change point detection. Higher values detect more frequent, potentially spurious changes.
        *   `min_segment_length`: Minimum allowed length of a time series segment.
        *   `max_segment_length`: Maximum allowed length of a time series segment.
    *   Output: A list of time series segments, each representing a contiguous portion of the original time series.  Each segment includes start and end indices within the original series.

2.  **Segment-Specific Feature Engineering:**
    *   Input: List of time series segments.
    *   Function:  Applies different feature engineering pipelines to segments based on segment length.
        *   Short segments (< 5 data points): Simple statistical features (mean, std. dev, min, max).
        *   Medium segments (5-20 data points): Rolling statistics, trend analysis (linear regression slope).
        *   Long segments (>20 data points):  Fourier transform (frequency domain features), wavelet decomposition.
    *   Output:  Feature vectors for each time series segment.

3.  **Dynamic Routing & Algorithm Selection:**
    *   Input: Feature vectors for each segment, training specification.
    *   Function:
        *   **Segment Classification:** Train a meta-classifier (e.g., Random Forest) to predict the best learning algorithm for each segment based on its features. The meta-classifier is trained on labeled data indicating which algorithm performed best on similar segments during a validation phase.
        *   **Algorithm Pool:** Maintain a pool of diverse learning algorithms (e.g., ARIMA, LSTM, Prophet, XGBoost).
        *   **Routing Logic:**  Route each segment to the learning algorithm predicted by the meta-classifier.
    *   Output:  A set of trained learning algorithms, each specialized to a particular type of time series segment.

4.  **Prediction & Aggregation:**
    *   Input: New time series data to predict, trained learning algorithms.
    *   Function:
        *   **Segment Detection:**  Apply the same change point detection algorithm used during training to identify segments in the new data.
        *   **Algorithm Application:** Route each segment to the corresponding specialized learning algorithm.
        *   **Weighted Ensemble:** Combine the predictions from each algorithm using a weighted average. Weights can be determined by the meta-classifier’s confidence in the algorithm selection.
        *   **Aggregation Directives Integration:** Incorporate the aggregation directives from the prediction specification as previously described (percentiles, temporal conditions).
    *   Output: Probabilistic prediction of future values.

**Pseudocode (Prediction Phase):**

```pseudocode
function predict(newTimeSeries, trainedAlgorithms, predictionSpecification):
  segments = detectSegments(newTimeSeries)
  predictions = []
  for segment in segments:
    algorithm = selectAlgorithm(segment, trainedAlgorithms) // Based on segment features
    prediction = algorithm.predict(segment)
    predictions.append(prediction)

  # Apply aggregation directives from predictionSpecification
  aggregatedPrediction = combinePredictions(predictions, predictionSpecification)
  return aggregatedPrediction
```

**Novelty:** The key innovation is the *dynamic* decomposition and routing. Unlike static segmentation, this approach adapts to the characteristics of each individual time series, enabling a highly flexible and specialized ensemble. This moves beyond simply training different algorithms on the same data, to tailoring *both* the data preparation and the algorithm selection.