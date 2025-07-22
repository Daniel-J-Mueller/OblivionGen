# 11706146

## Dynamic Sub-Path Stitching with Predictive Capacity Adjustment

**Concept:** Enhance the described system by allowing local network managers to *dynamically stitch together* sub-paths from multiple candidate paths, rather than being restricted to a single path or percentage distribution across pre-defined paths. This is coupled with a predictive capacity adjustment system based on real-time and historical data.

**Specifications:**

**1. Data Collection & Prediction Module (Local Network Manager):**

*   **Real-time Link Capacity Monitoring:** Continuously monitor bandwidth *and* latency (jitter, packet loss) on all intra-data center links.
*   **Historical Traffic Patterns:** Store historical traffic matrices (source-destination pairs, time of day, day of week) for the local data center.
*   **Predictive Modeling:** Employ a time-series forecasting model (e.g., ARIMA, LSTM) to predict future link capacity and traffic demands.  Model inputs: real-time data, historical data, global network load information (received from Global Network Manager). Output: Predicted capacity & demand for each link over a short time horizon (e.g., 5-15 minutes).
*   **Capacity Degradation Factor:** Calculate a 'Capacity Degradation Factor' for each link, reflecting the likelihood of congestion based on predictive modeling.

**2. Sub-Path Generation & Stitching (Local Network Manager):**

*   **Sub-Path Database:** Each Local Network Manager maintains a database of pre-calculated sub-paths within its data center. Sub-paths represent routes between key switching nodes.
*   **Candidate Path Decomposition:** When receiving candidate paths from the Global Network Manager, decompose each path into a sequence of sub-paths within the local data center.
*   **Dynamic Sub-Path Selection:**  Instead of assigning a fixed percentage to a pre-defined path, the Local Network Manager selects the *optimal* sequence of sub-paths based on:
    *   Current link capacity.
    *   Predicted link capacity (using Capacity Degradation Factor).
    *   Cost function:  Minimize weighted sum of latency & predicted congestion probability.
*   **Stitching Algorithm:**  Algorithm iteratively builds paths by selecting the lowest-cost sub-path, considering the predicted load on subsequent links. 

**3. Global Coordination & Feedback:**

*   **Load Reporting:** Local Network Managers periodically report aggregated link load and predicted congestion to the Global Network Manager.
*   **Global Path Recalculation:** The Global Network Manager uses this feedback to refine its candidate path calculations and provide more accurate recommendations.
*   **Adaptive Weighting:** The Global Network Manager adjusts the weighting of latency vs. bandwidth in its path ranking algorithm based on overall network conditions.

**4. Pseudocode (Local Network Manager - Sub-Path Selection):**

```
function select_sub_path(destination_node, current_node, candidate_sub_paths):
  best_path = null
  min_cost = infinity

  for sub_path in candidate_sub_paths:
    next_node = sub_path.destination
    link_capacity = get_current_link_capacity(current_node, next_node)
    predicted_capacity = get_predicted_link_capacity(current_node, next_node)
    congestion_probability = 1 - (predicted_capacity / link_capacity)

    cost = (sub_path.latency * weight_latency) + (congestion_probability * weight_congestion)

    if cost < min_cost:
      min_cost = cost
      best_path = sub_path

  return best_path
```

**5. Hardware Considerations:**

*   Increased processing power at local network managers to support real-time data analysis and predictive modeling.
*   High-bandwidth links for reporting aggregated load information to the Global Network Manager.

**Novelty:** This system moves beyond static path selection or percentage distribution by enabling *dynamic* path construction based on localized predictions. This allows for a more responsive and efficient network that can adapt to changing conditions in real-time.  The predictive modeling component further enhances the system's ability to anticipate and avoid congestion.