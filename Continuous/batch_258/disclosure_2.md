# 10977685

## Dynamic Canonical Identifier Weighting & Predictive Resolution

**Concept:** The existing patent focuses on resolving identifiers and constructing canonical IDs. This builds on that by introducing *dynamic weighting* to different identifier types within the resolution process and implementing *predictive* canonical ID generation based on user behavior patterns. This moves beyond simple tier-based hierarchy to a more nuanced, probabilistic system.

**Specs:**

**1. Identifier Weighting System:**

*   **Data Store:** A configuration data store containing weights for each identifier type (ad user ID, browser ID, SIS device ID, etc.). Weights are floating-point values ranging from 0.0 to 1.0, indicating the relative confidence in that identifier. These weights are *dynamic* and adjustable based on observed data (described in section 3).
*   **Weight Assignment Logic:**
    *   Each incoming identifier is assigned the weight defined in the data store.
    *   During canonical ID construction, identifiers are weighted proportionally. For example, if an SIS device ID has a weight of 0.8 and a browser ID has a weight of 0.2, the SIS ID contributes 80% to the overall confidence score for that user.
*   **Combined Confidence Score:** A combined confidence score is calculated for each possible canonical ID based on the weighted identifiers. The canonical ID with the highest score is selected.

**2. Predictive Canonical ID Generation:**

*   **Behavioral Data Collection:** Collect user behavior data, including:
    *   Frequency of app/browser usage.
    *   Time since last activity.
    *   Geographic location.
    *   Conversion events.
*   **Predictive Model:** Train a machine learning model (e.g., a recurrent neural network) to predict the likelihood of a user switching between devices or browsers. The model should take behavioral data as input and output a probability distribution over possible device/browser combinations.
*   **Prediction-Enhanced Resolution:**
    *   When a new identifier is received, the predictive model is consulted to determine the most likely device/browser combination.
    *   The resolution process prioritizes canonical IDs that align with the model's predictions.

**3. Dynamic Weight Adjustment:**

*   **Feedback Loop:** Implement a feedback loop that monitors the accuracy of the resolution process.
*   **Accuracy Metrics:** Track metrics such as:
    *   Rate of misattributed conversions.
    *   Number of duplicate user profiles.
*   **Weight Adjustment Algorithm:**
    *   If the accuracy metrics fall below a certain threshold, the algorithm automatically adjusts the identifier weights.
    *   Weights are adjusted proportionally to the observed error rate for each identifier type.
    *   For example, if misattributed conversions are frequently associated with browser IDs, the weight for browser IDs is reduced, and the weight for more reliable identifiers (e.g., SIS device IDs) is increased.

**Pseudocode - Canonical ID Construction with Weighting:**

```
function constructCanonicalID(identifiers):
  // Retrieve weights for each identifier type
  weights = getIdentifierWeights(identifiers)

  // Calculate weighted confidence score for each possible canonical ID
  scores = {}
  for each canonicalID in possibleCanonicalIDs(identifiers):
    score = 0
    for each identifier in identifiers:
      score += identifier.weight * identifier.confidence
    scores[canonicalID] = score

  // Select the canonical ID with the highest score
  bestCanonicalID = max(scores, key=scores.get)

  return bestCanonicalID
```

**Data Structures:**

*   **Identifier Object:** `{type: string, value: string, weight: float, confidence: float}`
*   **Configuration Data Store:**  A key-value store mapping identifier types to their weights.
*   **Predictive Model:** A trained machine learning model that predicts device/browser combinations.