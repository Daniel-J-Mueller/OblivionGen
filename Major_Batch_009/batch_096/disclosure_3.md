# 11775299

## Dynamic Vector Clock Granularity for Heterogeneous Execution

**Concept:** Extend the vector clock concept to operate not at a uniform node level, but at a *granularity level within nodes*. This allows finer-grained synchronization tracking, particularly beneficial for heterogeneous execution engines where different parts of a single node might be running on drastically different hardware (CPU, GPU, specialized accelerators).

**Problem Addressed:** The patent focuses on synchronizing *nodes* but doesn't account for the internal complexity of those nodes. A single node might contain sub-operations – matrix multiplication, activation function application, etc. – that are independently schedulable and could benefit from more precise synchronization signals.  Treating the entire node as a single synchronization unit limits potential concurrency.

**Specification:**

1.  **Node Decomposition:**  Each node in the computation graph is decomposed into a set of *execution granules*. An execution granule represents a unit of work suitable for execution on a specific execution engine.  Granules are defined during graph compilation.  Example: A node performing a convolution might be decomposed into granules for input fetching, kernel application (GPU), and output accumulation (CPU).

2.  **Granule Vector Clocks:** Each granule maintains its own vector clock. The dimension of the vector clock (n) remains greater than the total number of execution engines (m), but now it reflects the *potential* concurrency of granules across *all* nodes, not just the nodes themselves.

3.  **Granule Dependency Graph:** A ‘Granule Dependency Graph’ (GDG) is constructed *within* each node, detailing dependencies between the execution granules. This graph informs the vector clock updates.

4.  **Vector Clock Update Rules:**
    *   When a granule *completes* execution, it signals its completion to all downstream granules (as defined by the GDG) in *other* nodes.
    *   A downstream granule updates its vector clock based on the completion signal (and the completed granule’s vector clock) *and* its own GDG dependencies.  Specifically:
        *   Let `VC_i` be the vector clock of granule `i`.
        *   Let `VC_j` be the vector clock of a completed upstream granule `j`.
        *   Let `GDG_i` be the GDG for granule `i`.
        *   `VC_i = max(VC_i, VC_j + 1)` for each dependency edge from `j` to `i` in `GDG_i`.
        *   This ensures that a granule will not start executing until all its dependencies are met.

5.  **Event Register Allocation:** Event registers are allocated to *edges in the GDG*, not just edges between nodes. This allows for finer-grained tracking of dependencies and potential race conditions.

6.  **Redundancy Removal & Race Condition Detection:** Algorithms for removing redundant edges and detecting race conditions are adapted to operate on the GDG instead of the main computation graph.

**Pseudocode (Vector Clock Update):**

```
function update_vector_clock(current_granule, completed_granule):
  // current_granule: The granule being updated
  // completed_granule: The granule that has finished execution
  
  dependency_exists = check_dependency(current_granule.GDG, completed_granule)

  if dependency_exists:
    for i in range(len(current_granule.vector_clock)):  // Iterate through vector clock dimensions
      current_granule.vector_clock[i] = max(current_granule.vector_clock[i], completed_granule.vector_clock[i] + 1)
```

**Hardware Considerations:**

*   Requires a mechanism for efficient signal propagation between execution engines.
*   Increased memory overhead due to storing vector clocks for individual granules.
*   Potential for increased hardware complexity to manage the finer-grained synchronization.