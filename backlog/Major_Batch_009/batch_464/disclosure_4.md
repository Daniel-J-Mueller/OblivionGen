# 12153576

## Adaptive Disjunct Re-weighting for Query Optimization

**Specification:**

**I. Overview:**

This design details a system for dynamically adjusting the estimated cost/benefit of disjuncts within a disjunctive join condition *during* query execution.  Instead of relying solely on static cost estimations during query planning, the system monitors actual runtime statistics of each disjunct and re-weights their influence on the overall query plan. This adapts to skewed data distributions or unexpected access patterns.

**II. Core Components:**

*   **Disjunct Monitor:**  A module embedded within the query execution engine that tracks the following for each disjunct:
    *   Number of rows returned.
    *   Execution time.
    *   I/O operations performed.
    *   CPU utilization.
*   **Cost Model Adaptor:** This component receives data from the Disjunct Monitor and modifies the cost estimations associated with each disjunct in the current query plan.
*   **Plan Switcher:** Based on the adapted cost estimations, this module dynamically switches between different execution plans for the disjunctive condition.  Possible plans include:
    *   **Original Plan:** The plan initially determined during query planning.
    *   **Disjunct Prioritization:** Re-orders the disjuncts in the join to evaluate the most promising ones first.
    *   **Early Termination:**  If one disjunct yields a sufficient number of results, the other disjuncts can be skipped.
    *   **Plan Splitting:**  Executes different disjuncts in parallel using separate execution contexts.

**III.  Algorithm (Pseudocode):**

```
// Initialization (at query start)
original_cost_estimates = GetCostEstimatesFromQueryPlan()
disjunct_stats = {}  // Stores runtime stats for each disjunct

// During query execution (for each disjunct)
function UpdateDisjunctStats(disjunct_id, rows_returned, execution_time, io_operations, cpu_utilization) {
    disjunct_stats[disjunct_id] = {
        rows: rows_returned,
        time: execution_time,
        io: io_operations,
        cpu: cpu_utilization
    }
}

function ReWeightDisjuncts(disjunct_stats, original_cost_estimates) {
    weighted_costs = {}
    for (disjunct_id in original_cost_estimates) {
        if (disjunct_stats[disjunct_id]) {
            // Calculate a runtime-based cost factor
            runtime_factor = (disjunct_stats[disjunct_id].time * disjunct_stats[disjunct_id].io) / (disjunct_stats[disjunct_id].rows + 1) // Avoid division by zero

            //Adjust cost. Higher runtime factor = higher cost.
            weighted_costs[disjunct_id] = original_cost_estimates[disjunct_id] * (1 + runtime_factor)
        } else {
            weighted_costs[disjunct_id] = original_cost_estimates[disjunct_id] //Use original if stats unavailable
        }
    }
    return weighted_costs
}

function SelectExecutionPlan(weighted_costs) {
    //Logic to determine plan. Example:
    if (weighted_costs[disjunct_1] < weighted_costs[disjunct_2] * 0.5) {
        return "Prioritize Disjunct 1"
    } else if (TotalRowsReturned(disjunct_1) > target_row_count) {
        return "Early Termination - Disjunct 1 Sufficient"
    } else {
        return "Original Plan"
    }
}

//Main Execution Loop
For each disjunct in disjunctive condition:
    Execute Disjunct
    UpdateDisjunctStats(disjunct_id, rows_returned, execution_time, io_operations, cpu_utilization)
    weighted_costs = ReWeightDisjuncts(disjunct_stats, original_cost_estimates)
    selected_plan = SelectExecutionPlan(weighted_costs)
    AdaptQueryPlan(selected_plan)
```

**IV. Data Structures:**

*   `DisjunctStats`: A dictionary or hashmap storing runtime statistics for each disjunct.
*   `WeightedCosts`: A dictionary or hashmap storing the adjusted cost estimations for each disjunct.
*   `QueryPlanAdaptor`:  An interface or abstraction for modifying the query execution plan dynamically.

**V. Implementation Considerations:**

*   **Overhead:**  Minimize the overhead of monitoring and re-weighting to avoid negating performance gains.
*   **Sampling:** Use sampling techniques to reduce the number of rows processed for monitoring.
*   **Thresholds:**  Define appropriate thresholds for triggering plan adaptation.
*   **Concurrency:**  Ensure thread safety when updating and accessing statistics.
*   **Integration:**  Integrate seamlessly with existing query optimization and execution engines.