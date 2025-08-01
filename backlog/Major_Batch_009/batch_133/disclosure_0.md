# 12190081

## Dynamic Code Smell Detection & Refactoring Suggestion System

**Specification:**

**I. Core Functionality:**

This system extends the concept of session-specific code recommendations by actively identifying “code smells” *during* the editing session and providing targeted refactoring suggestions, leveraging a continuously updated machine learning model.  It's not simply suggesting alternatives for *existing* edits, but proactively detecting potential issues.

**II. Components:**

*   **Real-time Code Analysis Engine:**  Monitors code changes as the developer types.  This engine isn't limited to syntax errors; it uses a trained ML model to identify established code smells (e.g., long methods, duplicate code, feature envy, data clumps).
*   **Dynamic Smell Profile:**  Maintains a running profile of identified code smells *specific to the current editing session*. This profile weights smells based on frequency and severity.
*   **Refactoring Recommendation Engine:**  Based on the Dynamic Smell Profile, this engine proposes targeted refactoring suggestions. These suggestions aren't generic; they are tailored to the context of the current code and the developer’s recent activity.
*   **Session-Specific ML Model:** A neural network trained on a combination of:
    *   A large corpus of well-written code (initial training).
    *   The developer's coding style *within the current session*.  (Fine-tuned with each edit.)
    *   Historical data from similar projects or developers (optional, for broader context).
*   **Interactive UI Integration:** Presents refactoring suggestions directly within the IDE.  Suggestions include:
    *   A clear description of the code smell.
    *   A preview of the refactored code.
    *   A confidence score (indicating the model's certainty).
    *   Options to accept, reject, or ignore the suggestion.

**III. Pseudocode:**

```
// Main Loop (runs continuously in the IDE)
function onCodeChange(code) {
  smells = analyzeCode(code); // Returns a list of detected code smells with severity
  updateDynamicSmellProfile(smells);

  recommendations = generateRefactoringRecommendations(dynamicSmellProfile);

  displayRecommendationsInIDE(recommendations);
}

function analyzeCode(code) {
  // Uses the session-specific ML model to identify code smells.
  // Returns a list of Smell objects: {type: "LongMethod", severity: 0.8, location: {line: 10, char: 5}}
}

function updateDynamicSmellProfile(smells) {
  // Updates the dynamic profile based on new smells.
  // Weights smells based on frequency, severity, and recent edits.
}

function generateRefactoringRecommendations(dynamicSmellProfile) {
  // Generates refactoring suggestions based on the dynamic profile.
  // Prioritizes suggestions based on impact and confidence.
}

function displayRecommendationsInIDE(recommendations) {
  // Displays recommendations in a user-friendly way within the IDE.
  // Allows the user to accept, reject, or ignore suggestions.
}

//Model Training loop (runs in background)
function trainModel(newEdit){
    //incorporate edit into training data
    //retrain neural net
}
```

**IV. Novelty:**

This system shifts from *reactive* recommendation (suggesting alternatives to what the developer already did) to *proactive* detection and suggestion.  The continuous training of the model *within the session* allows it to adapt to the developer’s unique style and the specific context of the project, making recommendations far more relevant and effective. This goes beyond static code analysis and embraces dynamic learning during the act of coding.