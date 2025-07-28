# 11308026

## Adaptive Granularity Systolic Array

**Concept:** Extend the row-oriented bus concept to allow *dynamic* reconfiguration of bus width and data flow granularity within the systolic array *during* operation. This moves beyond fixed-width buses servicing predetermined processing element pairs, allowing for efficient handling of varying data dependencies and algorithm-specific parallelism.

**Specs:**

*   **Array Architecture:** Maintain the core systolic array structure (rows & columns of processing elements).
*   **Dynamic Bus Interconnect:** Each processing element (PE) has configurable interconnects. Instead of hardwired row-oriented buses, each PE possesses programmable switches allowing it to connect to *any* other PE within a defined radius (e.g., a 3x3 or 5x5 neighborhood).
*   **Granularity Control:** Each PE has a 'granularity' setting. This setting defines the amount of data it operates on in a single cycle. A granularity of 1 means it processes a single data element. A granularity of 2, 4, 8, etc., means it operates on a vector or chunk of data.
*   **Control Network:** A dedicated control network overlays the array. This network is responsible for:
    *   Configuring the interconnects between PEs.
    *   Setting the granularity of each PE.
    *   Synchronizing data flow across the reconfigured array.
*   **Data Format:** Support for variable-width data types. The array must be able to handle single-precision floats, double-precision floats, integers, etc.

**Operation:**

1.  **Algorithm Mapping:** Before execution, the target algorithm is mapped onto the array. This process determines the optimal data flow and processing element assignments.
2.  **Configuration Phase:** The control network configures the interconnects and granularity settings based on the algorithm mapping. This phase may involve creating multiple, dynamically sized buses within a row or even across rows.
3.  **Execution Phase:** Data flows through the reconfigured array. Processing elements operate on the received data based on their granularity settings.

**Pseudocode (Configuration Phase):**

```
// Algorithm Mapping results in 'data_dependencies' & 'PE_assignments'

FOR EACH row IN array:
    FOR EACH PE IN row:
        // Determine optimal connectivity based on 'data_dependencies' and 'PE_assignments'
        neighbors = find_neighbors(PE, data_dependencies)

        // Configure interconnects to connect to neighbors
        configure_interconnects(PE, neighbors)

        // Determine optimal granularity based on 'data_dependencies'
        granularity = calculate_granularity(PE, data_dependencies)

        // Set PE granularity
        set_granularity(PE, granularity)

// Synchronize configuration across the array
synchronize_configuration()
```

**Potential Benefits:**

*   **Algorithm Flexibility:** Supports a wider range of algorithms than traditional systolic arrays.
*   **Performance Optimization:** Can adapt to data dependencies and optimize performance for specific algorithms.
*   **Resource Utilization:** Efficiently utilizes processing elements and interconnects.
*   **Fault Tolerance:** Reconfigurable interconnects can potentially bypass faulty processing elements.