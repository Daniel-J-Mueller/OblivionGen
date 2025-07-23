# 11461662

## Dynamic Subgraph Merging & Splitting

**Specification:** Implement a system for dynamically merging and splitting subgraphs during compilation, guided by real-time performance monitoring during a preliminary 'shadow' execution phase. This moves beyond static subgraph analysis.

**Core Concept:** Instead of pre-defined subgraphs, the system begins with a graph representing the entire neural network.  During a short ‘shadow’ run with representative data (a small batch), the system monitors compute and memory usage *at the operator level*. Based on this live data, it dynamically merges operators into larger subgraphs if they exhibit complementary resource usage patterns (e.g., one is compute-bound, the other memory-bound).  Conversely, it splits operators if they become bottlenecks, even if initially part of a larger subgraph.

**Data Structures:**

*   `OperatorNode`:  Represents a single operator in the neural network. Contains:
    *   `compute_usage`:  Average compute usage during the shadow run.
    *   `memory_usage`: Average memory usage during the shadow run.
    *   `latency`: Average execution latency during the shadow run.
    *   `subgraph_id`: Identifier of the subgraph the operator currently belongs to.
*   `SubGraph`: Represents a subgraph. Contains:
    *   `operator_list`: List of `OperatorNode` IDs belonging to the subgraph.
    *   `total_compute_usage`: Sum of `compute_usage` for all operators.
    *   `total_memory_usage`: Sum of `memory_usage` for all operators.
    *   `total_latency`: Sum of `latency` for all operators.
*   `Graph`: Represents the overall neural network. Contains:
    *   `operator_map`:  Dictionary mapping operator ID to `OperatorNode`.
    *   `subgraph_map`: Dictionary mapping subgraph ID to `SubGraph`.

**Algorithm:**

1.  **Initialization:** Create `OperatorNode` for each operator in the neural network and assign each to its own initial `SubGraph`.
2.  **Shadow Execution:** Run a small batch of representative data through the neural network. Monitor `compute_usage`, `memory_usage`, and `latency` for each `OperatorNode`.
3.  **Subgraph Merging Phase:**
    *   Iterate through all pairs of adjacent `SubGraph`s.
    *   Calculate a ‘complementarity score’ based on the ratio of `total_compute_usage` to `total_memory_usage` for each subgraph.  A large difference in ratios indicates potential complementarity.
    *   If the complementarity score exceeds a threshold:
        *   Merge the two `SubGraph`s into a single `SubGraph`.
        *   Update `subgraph_map` and `OperatorNode` to reflect the new subgraph assignment.
4.  **Subgraph Splitting Phase:**
    *   Iterate through each `SubGraph`.
    *   Calculate the ‘bottleneck score’ for each `OperatorNode` within the subgraph: `bottleneck_score = (latency / total_latency) * (compute_usage + memory_usage)`.
    *   If the `bottleneck_score` exceeds a threshold:
        *   Split the `SubGraph` into two `SubGraph`s, with the bottleneck operator as the dividing point.
        *   Update `subgraph_map` and `OperatorNode` to reflect the new subgraph assignments.
5.  **Iteration:** Repeat steps 3 and 4 for a fixed number of iterations, or until the subgraph configuration stabilizes (i.e., the number of merges/splits falls below a threshold).
6.  **Compilation:** Compile the neural network based on the final subgraph configuration, applying optimizations appropriate for each subgraph (e.g., compute optimizations for compute-bound subgraphs, memory optimizations for memory-bound subgraphs).

**Pseudocode (Subgraph Merging):**

```
function merge_subgraphs(subgraph_map, operator_map, merge_threshold):
  for subgraph_id1, subgraph1 in subgraph_map.items():
    for subgraph_id2, subgraph2 in subgraph_map.items():
      if subgraph_id1 != subgraph_id2:
        # Check if subgraphs are adjacent (e.g., share an operator as input/output)
        if are_adjacent(subgraph1, subgraph2):
          complementarity_score = calculate_complementarity(subgraph1, subgraph2)
          if complementarity_score > merge_threshold:
            merge_subgraphs(subgraph_id1, subgraph_id2, subgraph_map, operator_map)
```

**Potential Benefits:**

*   Improved compilation speed by reducing the number of static subgraphs.
*   More accurate identification of compute and memory bottlenecks.
*   Increased flexibility to adapt to different hardware platforms and model configurations.
*   Dynamic optimization based on observed runtime behavior.