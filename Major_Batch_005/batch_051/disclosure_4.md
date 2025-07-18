# 10902062

## Adaptive Dimensional Weighting via Simulated Annealing

**Concept:** Expand upon the random cut tree approach by introducing a mechanism for *dynamically* weighting dimensions during tree construction and traversal. Instead of treating all dimensions equally, the system will employ a simulated annealing algorithm to adjust dimensional weights, prioritizing dimensions exhibiting higher anomaly contribution based on preliminary data analysis. This will result in trees that are more sensitive to relevant dimensions and less susceptible to noise.

**Specs:**

1.  **Dimensional Weight Initialization:**
    *   Each dimension begins with an equal weight (1.0).
    *   A “temperature” parameter (`T`) is initialized (e.g., 100.0).
    *   A “cooling rate” parameter (`alpha`) is defined (e.g., 0.95).
2.  **Tree Construction Phase:**
    *   During random cut tree construction, when selecting a dimension for a split, a weighted random selection is performed. The probability of selecting a dimension `i` is proportional to its current weight `w_i`.
    *   After each split, a "cost function" is evaluated. This function measures the variance within the resulting child nodes. Lower variance indicates a better split. A basic cost function:

        ```
        cost = sum(variance(child_node_j))
        ```

    *   If the new split (using the weighted random selection) *decreases* the cost, the change is accepted.
    *   If the new split *increases* the cost, a probabilistic acceptance criterion is applied:

        ```
        acceptance_probability = exp(-(new_cost - old_cost) / T)
        ```

        A random number between 0 and 1 is generated. If the random number is less than the `acceptance_probability`, the new split is accepted, *even though it increased the cost*. This allows the algorithm to escape local optima.

    *   After evaluating the split, dimensional weights are updated.
        *   Dimensions used in “good” splits (accepted splits) have their weights increased.
        *   Dimensions used in “bad” splits (rejected splits) have their weights decreased.  
        *   Weight adjustment formula: `w_i = w_i + learning_rate * (cost_change)` where `learning_rate` is a tunable hyperparameter.
3.  **Traversal & Anomaly Scoring:**
    *   When traversing the tree with a data point, the anomaly contribution calculation within each node is adjusted by the dimension's current weight. Dimensions with higher weights have a greater influence on the overall anomaly score.
    *   During traversal, record the accumulated weight for each dimension.  This could be used as a feature in a downstream model.
4.  **Temperature Cooling:**
    *   After a set number of tree constructions or a certain time interval, the temperature `T` is reduced:  `T = T * alpha`. This gradually reduces the probability of accepting worse splits, guiding the algorithm towards a more stable configuration.
5.  **Parameter Tuning:**
    *   `learning_rate`: Controls the magnitude of weight adjustments.
    *   `alpha`: Cooling rate.
    *   Initial `T`.
    *   Number of tree constructions per cooling cycle.

**Pseudocode (Dimensional Weight Update):**

```python
def update_dimensional_weights(dimension_used, cost_change, learning_rate, weights):
    weights[dimension_used] = weights[dimension_used] + learning_rate * cost_change
    return weights
```

**Potential Improvements:**

*   **Adaptive Learning Rate:** Adjust the `learning_rate` based on the rate of cost reduction.
*   **Multi-Objective Optimization:** Incorporate other objectives into the cost function, such as tree depth or node size.
*   **Ensemble of Trees:** Construct multiple trees with different parameter settings and combine their anomaly scores.