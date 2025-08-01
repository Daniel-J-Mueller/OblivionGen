# 10977685

**Adaptive Identifier Decay & Probabilistic Merging**

**Core Concept:** Enhance canonical identifier resolution by incorporating a decay mechanism for identifiers based on inactivity and employing probabilistic merging strategies when faced with conflicting identity information. This addresses scenarios where user behavior is inconsistent or fragmented, leading to more accurate and robust identity resolution.

**Specification:**

1.  **Identifier Decay Mechanism:**
    *   Each identifier (ad user ID, browser ID, device ID, user account ID) will be assigned a "decay score."
    *   Decay score starts at 1.0 (fully valid).
    *   Inactivity (lack of associated events – clicks, conversions, views) reduces the decay score over time. A configurable decay rate (e.g., 0.99 per day of inactivity) will be applied.
    *   New activity resets the decay score to 1.0.
    *   A configurable threshold (e.g., 0.2) will define an identifier as "stale." Stale identifiers contribute less weight to merging decisions.

2.  **Probabilistic Merging Algorithm:**
    *   When resolving identities, calculate a "merge probability" based on the following factors:
        *   **Decay Score Similarity:** Higher similarity in decay scores between identifiers increases merge probability.
        *   **Event Overlap:** Calculate the overlap in events (within a configurable time window) associated with different identifiers. Higher overlap increases merge probability.
        *   **Data Source Trust:** Assign a "trust score" to different data sources (e.g., SIS, data service feeds).  Identifiers originating from higher-trust sources contribute more weight to the merge probability.
        *   **Identifier Type Hierarchy:** Leverage the existing identifier hierarchy, but adjust weights based on the decay score.  A stale high-tier identifier might be given less weight than a fresh low-tier identifier.
    *   The merge probability will be calculated using a weighted sum of these factors, normalized to a value between 0 and 1.
    *   A configurable threshold (e.g., 0.7) will determine whether to merge the identities. If the probability exceeds the threshold, the identities are merged into a single canonical identifier.

3.  **Canonical Identifier Mapping Updates:**
    *   When identities are merged, update the canonical identifier mapping accordingly.
    *   Track the merging history for auditing and debugging purposes.
    *   Implement a rollback mechanism to revert merging decisions if necessary.

4.  **System Components:**
    *   **Decay Score Manager:** Responsible for tracking and updating decay scores for all identifiers.
    *   **Merge Probability Calculator:**  Implements the merge probability algorithm.
    *   **Mapping Updater:**  Updates the canonical identifier mapping.
    *   **API Integration:**  Provides APIs for receiving identifier data, requesting identity resolution, and accessing the canonical identifier mapping.

**Pseudocode (Merge Probability Calculation):**

```
function calculateMergeProbability(identifier1, identifier2, eventOverlap, trustScore1, trustScore2, identifierHierarchyWeight):
  decayScore1 = getDecayScore(identifier1)
  decayScore2 = getDecayScore(identifier2)

  decayScoreSimilarity = 1 - abs(decayScore1 - decayScore2)  // Scale 0-1

  weightedEventOverlap = eventOverlap * 0.3
  weightedDecaySimilarity = decayScoreSimilarity * 0.4
  weightedTrust = (trustScore1 + trustScore2) / 2 * 0.3

  mergeProbability = weightedEventOverlap + weightedDecaySimilarity + weightedTrust

  return mergeProbability
```

**Novelty:**

This system moves beyond static identifier hierarchies and fixed merging rules. By incorporating decay and probability, it provides a more adaptable and robust solution for identity resolution in dynamic environments. It handles fragmented user behavior better, allowing more accurate attribution and conversion analysis. The weighting factors are configurable, allowing customization for specific data environments and business requirements.