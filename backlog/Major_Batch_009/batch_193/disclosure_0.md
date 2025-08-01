# 9819662

## Dynamic Authentication 'Echo' System

**Concept:** Expand the transaction history authentication beyond simple item recognition. Instead of *matching* items, build a system that learns the user's *pattern* of selection. This creates a dynamic ‘echo’ of their typical behavior, increasing security and usability.

**Specifications:**

**I. Data Acquisition & Profile Creation:**

1.  **Selection Logging:**  Beyond identifying *what* the user selects, log *how* they select it. Timestamp each selection, record mouse movement (or touch equivalent) leading up to the selection, and note the time spent considering each item.
2.  **'Selection Signature' Generation:** Create a ‘Selection Signature’ for each user. This is a multi-dimensional vector representing their selection behavior. Dimensions include:
    *   Average time per item considered.
    *   Variance in time spent per item.
    *   Typical scan path (represented as a sequence of item IDs).
    *   Ratio of items considered vs. items selected.
    *   Preference for visual elements (color, size, position) – derived from mouse tracking.
3.  **Dynamic Profile Update:** The Selection Signature isn’t static. It’s updated continuously with each authentication session.  A weighted average is used – recent sessions have a greater influence on the profile. (e.g., last 3 sessions weighted 60%, previous sessions 40%).

**II. Authentication Process:**

1.  **Challenge Generation:** Present the user with a set of items as in the original patent, but the item selection is now augmented with ‘decoys’. Decoys are items similar to those the user has previously interacted with, or items popular with other users.
2.  **Behavioral Capture:**  As the user makes their selection, continuously capture their behavioral data (timestamps, mouse movements, time spent on each item).
3.  **Signature Comparison:** Calculate a ‘Current Session Signature’ based on the captured behavioral data.
4.  **Authentication Metric:**  Compare the Current Session Signature to the user’s stored Selection Signature using a similarity metric (e.g., cosine similarity, Euclidean distance).
5.  **Thresholding & Multi-Factor Authentication:**
    *   If the similarity score exceeds a predefined threshold, the user is authenticated.
    *   If the score falls below the threshold, trigger a multi-factor authentication challenge (e.g., SMS code, biometric scan).
6. **Adaptive Thresholds:** Adjust the similarity threshold dynamically based on the risk profile of the transaction or the user’s historical behavior. (e.g. higher thresholds for high-value transactions)

**III. System Components:**

1.  **Behavioral Data Collector:** Software module responsible for capturing and logging user interaction data.
2.  **Signature Generator:** Module responsible for creating and updating Selection Signatures.
3.  **Similarity Engine:** Module responsible for comparing signatures and calculating similarity scores.
4.  **Authentication Manager:** Module responsible for managing the authentication process, triggering multi-factor authentication, and logging authentication events.
5.  **Database:** Secure storage for user profiles, transaction histories, and authentication logs.

**Pseudocode (Authentication Manager):**

```
function authenticateUser(userID, itemSet):
    behaviorData = captureUserBehavior(itemSet)
    currentSessionSignature = generateSignature(behaviorData)
    userProfile = loadUserProfile(userID)
    userSignature = userProfile.selectionSignature
    similarityScore = calculateSimilarity(userSignature, currentSessionSignature)

    if similarityScore > threshold:
        return AUTHENTICATED
    else:
        triggerMultiFactorAuthentication(userID)
        return REQUIRES_MFA
```

**Novelty:** This system moves beyond *what* the user selects to *how* they select it. By analyzing the user's behavioral patterns, it creates a more robust and user-friendly authentication method. This combats increasingly sophisticated phishing attacks which might present convincing item lists.