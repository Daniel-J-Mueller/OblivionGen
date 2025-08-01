# 12182549

## Adaptive Interference Graph Partitioning for Heterogeneous Memory

**Concept:** Dynamically partition the interference graph based on data access patterns *and* the heterogeneous nature of available memory (registers, L1 cache, L2 cache, DRAM, etc.). Instead of a uniform color assignment across the entire graph, introduce ‘memory domains’ within the graph.  The goal is to minimize data movement *between* memory domains, exploiting locality and reducing overall execution time.

**Specification:**

1.  **Memory Domain Definition:** 
    *   Identify available memory tiers (L1, L2, DRAM, etc.) and their associated access costs (latency, bandwidth).
    *   Create ‘memory domains’ corresponding to these tiers within the interference graph.  Each node in the graph is initially assigned to a primary memory domain (e.g., a variable is ‘home’ in L1 cache).
    *   Introduce ‘cross-domain edges’ representing dependencies between variables residing in different memory domains.

2.  **Dynamic Partitioning & Cost Modeling:**
    *   During the simplification process, after a spill candidate is selected, analyze the data access patterns (using profile information or static analysis) of the variable and its connected nodes.
    *   Calculate a ‘relocation cost’ for moving the spill candidate *and* its dependent nodes to a different memory domain. This cost should incorporate data transfer costs, potential cache conflicts, and performance impact.
    *   If the relocation cost is below a threshold, *move* the spill candidate and its immediate dependencies to a lower-cost memory domain.  This effectively ‘re-colors’ the nodes within the interference graph.

3.  **Rebuilding Process Adaptation:**
    *   During the rebuilding phase, when assigning colors (memory locations), prioritize colors *within* the current memory domain of each variable.
    *   Only consider assigning a variable to a different memory domain if all colors within its current domain are exhausted, *and* the relocation cost is acceptable.
    *   Implement a ‘domain balancing’ heuristic to ensure that no single memory domain becomes overly congested.

4.  **Color Selection Schemes (extended):**
    *   **Domain-Aware Reuse Scheme:**  Prioritize color reuse within the *same* memory domain before considering colors in other domains.
    *   **Adaptive Rotation Scheme:** Dynamically adjust the rotation speed and order based on the congestion level of each memory domain. Faster rotation for congested domains, slower rotation for less congested domains.

**Pseudocode (Simplified):**

```
function rebuild_graph(interference_graph, memory_domains):
  for each value in interference_graph:
    primary_domain = value.memory_domain
    available_colors = get_available_colors_in_domain(primary_domain)

    if available_colors is not empty:
      assign_color(value, select_color(available_colors))
    else:
      # Consider moving to a different domain
      relocation_cost = calculate_relocation_cost(value)
      if relocation_cost < threshold:
        move_value_to_new_domain(value)  # Update value.memory_domain
        available_colors = get_available_colors_in_domain(value.memory_domain)
        if available_colors is not empty:
          assign_color(value, select_color(available_colors))
        else:
          # Spill to DRAM (as a last resort)
          spill_to_dram(value)
```

**Hardware/Software Considerations:**

*   Requires hardware performance counters to accurately measure memory access costs.
*   Requires a compiler infrastructure capable of performing data access pattern analysis.
*   Can be integrated into existing register allocation and spilling algorithms.
*   Potential for performance gains on heterogeneous memory architectures (e.g., systems with high-bandwidth memory).