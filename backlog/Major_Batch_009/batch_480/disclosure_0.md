# 11669753

## Interactive Model ‘Genealogy’ & Drift Visualization

**Concept:** Extend the interactive interpretation system to not only analyze *current* model versions but to track and visualize the “genealogy” of model iterations – how changes propagate and ‘drift’ the model’s behavior over time. This allows users to pinpoint exactly when and why a model began making certain decisions, enabling more robust debugging and explainability.

**Specs:**

*   **Data Storage:**  A time-series database (e.g., InfluxDB, TimescaleDB) to store model versions. Each version stores:
    *   Model weights/parameters.
    *   Training data metadata (version, source).
    *   Performance metrics (accuracy, precision, recall).
    *   A “change log” – a record of what modifications were made to create this version from the previous one (e.g., “Weight adjustment: factor ‘X’ increased by 5%”, “Removed 10 records from dataset”).
    *   A ‘drift score’ – a metric quantifying the difference in behavior between this version and its predecessor (see ‘Drift Calculation’ below).

*   **UI/UX Extension:**
    *   A “Version History” panel within the existing interactive interpretation interface. This displays a timeline of model versions with key metrics and change logs visible.
    *   Version selection enables loading any past model version into the interpretation tools.
    *   A "Drift Visualization" pane:
        *   Displays the drift score over time (graph).
        *   Allows highlighting specific versions on the graph to see the changes made.
        *   Provides a "Diff View" of contributing factors between selected versions. The diff view highlights which factors have increased/decreased in importance and by how much.

*   **Drift Calculation:**  Employ a multi-faceted approach:
    1.  **Prediction Drift:** Run a representative test dataset through both the current and past versions. Calculate the percentage difference in predicted outcomes.
    2.  **Factor Importance Drift:** Compare the feature importance scores (e.g., from a feature importance plot) between versions. Calculate the mean absolute percentage difference.
    3.  **Decision Boundary Drift:** (For classification models) Calculate the change in decision boundary position in feature space. This can be done by sampling points near the boundary and observing the shift.
    4.  **Combined Drift Score:**  Weight each of the above drift metrics (user configurable) to create a single "drift score".

*   **Interactive ‘Rollback’:**  Allow users to “rollback” to a previous model version with a single click.  This should create a new version, preserving the rollback point for future reference.

*   **Attribution Analysis:** Implement a system that allows tracing a prediction back through the model genealogy. If a user queries a specific prediction from the *current* model, the system should show the prediction chain through all parent versions.  This allows users to see how a prediction evolved over time.

**Pseudocode (Attribution Analysis):**

```
function trace_prediction(prediction, current_version):
  version_history = get_version_history(current_version) //returns list of parent versions
  
  for parent_version in version_history:
    
    parent_prediction = load_model(parent_version).predict(input_data)
    
    print("Version: " + parent_version + ", Prediction: " + parent_prediction)
    
  print("Original Version Prediction: " + prediction)
```

This system enables not just *understanding* the current model, but also *understanding how it came to be*, fostering greater trust and control over AI systems.