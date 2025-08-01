# 8990199

## Dynamic Category Weighting via User Gaze Tracking

**Concept:** Adapt the visual similarity scoring within the search system by dynamically weighting categories based on where a user *looks* within displayed search results. This creates a feedback loop where the system learns user preferences in real-time and biases future results accordingly.

**Specifications:**

**1. Hardware Integration:**

*   **Eye Tracker:** Integrate an eye-tracking module (e.g., Tobii, Pupil Labs) with the display device presenting search results.
*   **Data Stream:** Establish a continuous data stream from the eye tracker providing:
    *   Gaze coordinates (x, y) on the display.
    *   Fixation events (duration exceeding a threshold).
    *   Saccade patterns (rapid eye movements).
*   **Calibration:** Implement a user-specific calibration routine for the eye tracker to ensure accuracy.

**2. Software Modules:**

*   **Result Layout Analysis:**  A module to dynamically analyze the layout of displayed search results. It needs to identify the bounding box coordinates of each result item (image, title, description) on the screen.
*   **Gaze-Result Mapping:** A module to determine which search result item the user is currently looking at.  This involves checking if the gaze coordinates fall within the bounding box of a result item.  A tolerance value should be configurable to account for minor inaccuracies in gaze tracking.
*   **Category Association:**  For each result item, identify the categories it belongs to within the existing category tree (as defined in the base patent).
*   **Dynamic Weight Adjustment:**
    *   **Weight Initialization:** Assign initial weights to all categories in the visually significant subset.
    *   **Fixation-Based Weighting:**  When a fixation event occurs on a result item:
        *   Increment the weight of the associated categories. The increment amount should be configurable.
        *   Decrement the weight of categories *not* associated with the result item (to prevent runaway weighting). The decrement amount should be smaller than the increment amount.
        *   Apply a decay function to weights over time (to reduce the influence of past fixations).
    *   **Weight Normalization:** After each weight update, normalize the weights of all categories to ensure they sum to 1.
*   **Modified Similarity Scoring:** Alter the existing visual similarity score calculation (Claim 1, “weighted based at least in part on a position of a category”) to incorporate the dynamically adjusted category weights.  The new formula would be:

    `Score = Σ (Similarity(Query, Result) * CategoryWeight(Category))`

    Where:

    *   `Similarity(Query, Result)` is the existing visual similarity score.
    *   `CategoryWeight(Category)` is the dynamically adjusted weight for the category the result belongs to.
*   **User Profile Management:** Store dynamically adjusted category weights in a user profile. This allows the system to personalize search results over time.

**3. Pseudocode:**

```
// Initialization
userProfile = LoadUserProfile()
categoryWeights = InitializeCategoryWeights(userProfile) // Or default if new user

// For each displayed search result item:
resultCategories = GetCategories(resultItem)

// During user interaction:
If (EyeTracker.FixationDetected()):
  gazeCoordinates = EyeTracker.GetGazeCoordinates()
  resultItem = FindResultItemAtCoordinates(gazeCoordinates)
  If (resultItem != NULL):
    For each category in resultCategories:
      categoryWeights[category] += WeightIncrement // Increase weight
    For each category not in resultCategories:
      categoryWeights[category] -= WeightDecrement // Decrease weight
    ApplyWeightDecay(categoryWeights)
    NormalizeWeights(categoryWeights)

// During similarity scoring:
For each result item:
  score = 0
  For each category in resultItem.categories:
    similarity = CalculateVisualSimilarity(query, result)
    score += similarity * categoryWeights[category]
  result.score = score
```

**4. Refinements:**

*   **Saccade Analysis:** Analyze saccade patterns to infer user intent. For example, a rapid saccade from one result to another might indicate a comparison.
*   **Heatmap Visualization:** Generate a heatmap of user gaze data to identify areas of interest within search results.
*   **A/B Testing:** Conduct A/B testing to evaluate the effectiveness of the dynamic weighting system compared to the original similarity scoring method.
*   **Contextual Awareness:** Incorporate contextual information (e.g., time of day, user location) to further refine the weighting system.