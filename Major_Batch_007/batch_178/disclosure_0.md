# 10553029

## Adaptive Fragment Prediction & Pre-Fetch

**Concept:** Leverage predicted user gaze and movement to proactively pre-fetch not just reference frames, but *entire fragments* of VR content, anticipating likely transitions between viewpoints. This moves beyond reference-only decoding to full fragment pre-loading, significantly reducing perceived latency.

**Specs:**

*   **Fragment Definition:**  VR content is segmented into time-coherent fragments (e.g., 500ms - 1 second duration) containing both base (monoscopic) and enhancement (stereoscopic) layers. These fragments are spatially divided into tiles or regions.
*   **Gaze Prediction Module:**  An AI model (likely LSTM or Transformer-based) trained on historical user gaze data, head movement, and scene geometry.  This module predicts the likely areas the user will be looking at within the next 100-200ms. Output is a probability distribution over spatial tiles.
*   **Movement Prediction Module:** Another AI model (similar architecture to gaze prediction) trained on head and body tracking data. Outputs a predicted trajectory and velocity.
*   **Priority Queue:** A data structure storing fragment tiles, prioritized based on:
    *   Gaze prediction probability
    *   Movement prediction proximity (how quickly the user is likely to reach the tile’s area)
    *   Distance from current viewing position (lower latency for closer tiles)
    *   Fragment download status (incomplete fragments receive higher priority).
*   **Pre-Fetch Mechanism:**  A background thread continuously pulls fragments from the priority queue and initiates downloads. Downloads are conducted using a bandwidth-aware scheduler (prioritizing higher-priority fragments and throttling during network congestion).
*   **Dynamic Resolution Scaling:** If bandwidth is limited, fragments can be downloaded at a lower resolution initially, with higher-resolution versions fetched later.
*   **Fragment Stitching:** A module responsible for seamlessly transitioning between pre-fetched fragments and ensuring visual coherence.

**Pseudocode (Pre-Fetch Loop):**

```
while (VR application is running):
  fragment = priorityQueue.pop()
  if (fragment.downloadStatus == incomplete):
    bandwidthAvailable = networkScheduler.checkBandwidth()
    if (bandwidthAvailable):
      downloadFragment(fragment, bandwidthAvailable)
    else:
      priorityQueue.push(fragment) //Re-queue for later
  if (fragment.isReady()):
    fragmentStitcher.addFragment(fragment)
```

**Innovation:**  Existing approaches focus on decoding reference frames.  This goes further by anticipating the *entire* viewpoint transition, pre-loading all necessary data, and dynamically adjusting resolution to maintain a smooth experience even with limited bandwidth.  It aims for *proactive* rendering, not just reactive decoding. The system is designed to "learn" a user's viewing habits and optimize pre-fetching accordingly.