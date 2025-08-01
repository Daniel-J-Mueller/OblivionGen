# 11556580

## Dynamic Keyframe Selection & Temporal Consistency

**Concept:** Enhance localization accuracy and reduce computational load by dynamically selecting keyframes *during* the image capture process, and enforcing temporal consistency across successive localizations.

**Specifications:**

**1. System Components:**

*   **Client Device:** Mobile device with camera, processing unit, and network connectivity.
*   **Edge Processing Unit:** (Optional) Lightweight processor on the client device or nearby infrastructure for initial frame analysis.
*   **Central Server:** Stores region data (key frames, latent vectors, keypoints, 3D mapping data).
*   **Temporal Buffer:**  A short-term memory on the client device/edge unit storing the last *N* frames (e.g., N=5-10) and their corresponding estimated locations.

**2. Dynamic Keyframe Generation & Selection:**

*   **Continuous Capture:** The client device camera continuously captures video frames (e.g., 10-15 fps).
*   **Feature Extraction:** The edge processing unit (or client CPU) extracts basic visual features (e.g., color histograms, edge density) from each frame.
*   **Novelty Detection:**  A novelty score is calculated for each frame, based on its deviation from the average feature vector of the previous *M* frames (e.g., M=3-5). This indicates how ‘different’ the current frame is.
*   **Keyframe Trigger:** If the novelty score exceeds a threshold, the frame is designated as a potential keyframe.
*   **Redundancy Filtering:**  A similarity check is performed between the potential keyframe and the last *K* keyframes (e.g., K=3-5). If the similarity exceeds a threshold, the frame is discarded to avoid redundant data.
*   **Keyframe Storage:** Accepted keyframes are temporarily stored for latent vector and keypoint extraction.

**3. Localization Process:**

*   **Feature Extraction:**  Latent vectors and keypoints are calculated from the *current* frame.
*   **Initial Localization:** The localization process proceeds as described in the parent patent, using the current frame's features to identify a matching region.
*   **Temporal Filtering:**
    *   Access the location estimates from the *N* previous frames in the Temporal Buffer.
    *   Calculate a weighted average of the current estimate and the previous estimates, giving higher weight to more recent estimates and/or estimates with higher confidence scores (derived from the similarity metrics).
    *   If the current estimate deviates significantly from the temporally filtered estimate (beyond a threshold), flag a potential localization error or ambiguity.

**4.  Adaptive Thresholding:**

*   Monitor the consistency of localization estimates over time.
*   Adjust the thresholds for novelty detection, redundancy filtering, and temporal consistency filtering based on the observed error rate and environmental conditions (e.g., lighting, GPS signal strength).

**Pseudocode (Temporal Filtering):**

```
function temporalFilter(currentEstimate, temporalBuffer):
  weightedSum = 0
  totalWeight = 0
  for i in range(len(temporalBuffer)):
    // Calculate weight based on age and confidence
    ageWeight = 1 / (i + 1) // Newer frames have higher weight
    confidenceWeight = temporalBuffer[i].confidence // Assume each entry has a confidence score
    weight = ageWeight * confidenceWeight

    weightedSum += temporalBuffer[i].location * weight
    totalWeight += weight

  filteredLocation = weightedSum / totalWeight
  return filteredLocation
```

**Potential Benefits:**

*   Reduced computational load on the server by transmitting fewer keyframes.
*   Improved localization accuracy by leveraging temporal consistency and filtering out spurious estimates.
*   Increased robustness to dynamic environments and temporary occlusions.
*   More efficient use of bandwidth and storage resources.