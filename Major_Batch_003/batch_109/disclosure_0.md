# 11748615

## Adaptive Operator Granularity via Dynamic Sub-Graph Partitioning

**Specification:** A system to dynamically adjust the granularity of operators within the stochastic supernet during the NAS process, and subsequently during inference, based on real-time performance metrics and hardware resource availability.

**Rationale:** The patent focuses on latency as a key metric for NAS. However, operator granularity (e.g., a large convolutional layer vs. a sequence of smaller ones) significantly impacts both latency *and* resource utilization (memory, power). Static granularity choices, even optimized via NAS, may not be optimal across diverse hardware platforms or changing workloads.

**System Components:**

1.  **Performance Monitoring Module:** Continuously monitors execution time, memory usage, and power consumption of individual operators *during training and inference*. This data is platform-specific.
2.  **Dynamic Partitioning Engine:**  This is the core component. It employs a graph partitioning algorithm (e.g., METIS, KaHyPar) to split or merge operators within the supernet's layers.  Partitioning/merging happens *during the NAS process*, altering the search space. This is driven by the performance metrics from the Monitoring Module.
3.  **Granularity Control Table (GCT):**  A lookup table storing optimal granularity configurations for different operator types, hardware platforms, and workload characteristics.  The NAS process *populates* the GCT.
4.  **Hardware Abstraction Layer (HAL):** Provides platform-specific details about available resources (memory, compute units, etc.) to the Partitioning Engine. This facilitates informed partitioning decisions.
5.  **NAS Integration Module:** Modifies the DNAS engine to incorporate the GCT and allow for granularity adjustments during the architectural search. The loss function now includes terms related to both latency *and* resource utilization.

**Pseudocode (Partitioning Engine):**

```pseudocode
FUNCTION PartitionOperator(operator, hardware_profile, performance_data):
  // hardware_profile contains info like available memory, compute units
  // performance_data contains execution time, memory usage of operator

  IF performance_data.memory_usage > hardware_profile.memory_limit:
    // Attempt to split operator into smaller sub-operators
    sub_operators = SplitOperator(operator) // Algorithm to split operator (e.g., depthwise separable convolutions)
    IF sub_operators != NULL:
      RETURN sub_operators
    ELSE:
      // Splitting failed - operator is too complex or resource limits are too strict
      RETURN NULL // Indicate failure to partition
  ENDIF

  IF performance_data.execution_time > latency_threshold:
    // Attempt to merge operator with neighboring operator
    merged_operator = MergeOperator(operator, neighboring_operator)
    IF merged_operator != NULL:
      RETURN merged_operator
    ELSE:
      // Merging failed
      RETURN operator
    ENDIF
  ENDIF

  RETURN operator // No partitioning/merging needed
END FUNCTION
```

**NAS Process Modification:**

1.  During NAS, the DNAS engine evaluates different architectural configurations *and* different granularity configurations for each layer.
2.  The partitioning engine is invoked to dynamically adjust operator granularity based on performance data collected during training.
3.  The loss function is modified to include a weighted sum of latency, resource utilization, and model accuracy.
4.  The GCT is populated with optimal granularity configurations for different operator types, hardware platforms, and workloads.

**Inference Deployment:**

1.  The GCT is used to determine the optimal granularity configuration for each operator based on the target hardware platform and workload characteristics.
2.  The model is deployed with the specified granularity configuration.

**Novelty:** This design moves beyond fixed operator granularity and introduces a dynamic, hardware-aware approach to NAS and inference. It allows the system to adapt to changing hardware constraints and workload characteristics, maximizing performance and resource utilization.  The partitioning engine's ability to *actively modify* the search space during NAS is a significant departure from existing methods.