# 10244270

## Dynamic Predictive Fragment Caching

**Concept:** Enhance adaptive bitrate streaming by pre-caching video fragments *predictively*, not just reactively, based on user interaction with the webpage *beyond* video playback. This leverages the “above-the-fold” download time concept but extends it to *all* webpage interactions.

**Specifications:**

1.  **Interaction Tracking:** Implement a Javascript library to monitor *all* user interactions on the webpage – scrolling, mouse movements, link hovers, button presses, form inputs, etc. Assign a "weight" to each interaction type reflecting its potential correlation with upcoming video viewing. (e.g., scrolling down = higher weight, hovering over a non-video link = lower weight).
2.  **Interaction Vector:** Create an "Interaction Vector" – a continuously updating numerical representation of the user’s engagement with the webpage. This is a weighted sum of all tracked interactions over a sliding time window (e.g., last 5 seconds).
3.  **Fragment Dependency Graph:**  During manifest parsing, construct a "Fragment Dependency Graph". This graph represents the relationships between video fragments, considering segment durations and potential stitching points. Fragments with higher potential for immediate playback (e.g., the next segment) have stronger connections.
4.  **Predictive Caching Algorithm:**
    *   **Baseline:** Initially, cache the first few fragments based on the "above-the-fold" download time approach, as in the original patent.
    *   **Interaction-Driven Prediction:**  As the Interaction Vector changes, *predict* the probability of the user requesting *specific* fragments based on the Fragment Dependency Graph and the Interaction Vector.
    *   **Caching Priority:** Prioritize caching fragments with a high probability *and* a low current buffer level.
    *   **Dynamic Adjustment:**  Continuously adjust the caching priority and fragment selection based on the ongoing Interaction Vector and observed user behavior.
5. **Buffer Management:** Implement a "speculative buffer". This is a separate buffer that holds pre-cached fragments predicted by the algorithm. If the user requests a pre-cached fragment, it's served immediately from the speculative buffer. Otherwise, it remains available for future requests.
6. **Network Awareness:** Integrate with the existing adaptive bitrate algorithm to consider network conditions *in addition* to the Interaction Vector when making caching decisions. (e.g., if bandwidth is limited, prioritize caching lower-bitrate fragments).

**Pseudocode (Caching Logic):**

```
function updateCache(interactionVector, fragmentDependencyGraph, networkBandwidth, currentBuffer) {

  // Calculate probability of requesting each fragment
  fragmentProbabilities = calculateFragmentProbabilities(interactionVector, fragmentDependencyGraph);

  // Calculate cache priority for each fragment
  for (each fragment in fragmentDependencyGraph) {
    cachePriority = fragmentProbabilities[fragment] * (1 - currentBufferLevel[fragment]) * networkBandwidthFactor;
  }

  // Sort fragments by cache priority
  sortedFragments = sortFragmentsByPriority(fragments);

  // Cache the top N fragments (based on available buffer space)
  for (i = 0; i < N; i++) {
    if (availableBufferSpace > fragmentSize[sortedFragments[i]]) {
      cacheFragment(sortedFragments[i]);
      availableBufferSpace -= fragmentSize[sortedFragments[i]];
    }
  }
}

```

**Potential Benefits:**

*   Reduced buffering and improved playback smoothness.
*   Proactive adaptation to user viewing patterns.
*   Enhanced user experience.
*   More efficient bandwidth utilization.