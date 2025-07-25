# 11308026

## Dynamic Reconfiguration of Bus Topology

**Concept:** Implement a systolic array where the interconnection of row-oriented buses isn’t fixed at design time, but dynamically reconfigurable during operation. This allows the array to adapt its dataflow to optimize for different kernel structures or input data characteristics, significantly enhancing flexibility.

**Specs:**

*   **Array Structure:** Systolic array of processing elements (PEs) arranged in rows and columns, identical to the base patent.
*   **Reconfigurable Interconnect:** Each row incorporates a network of programmable switches connecting PEs to multiple row-oriented buses. This network is controlled by a local configuration controller within each row.
*   **Configuration Controller:**  Each row’s controller receives configuration data from a global configuration unit. This data specifies how PEs are connected to buses within that row.
*   **Configuration Data Format:** Configuration data is a bit vector. Each bit corresponds to a switch in the row's network. '1' activates a connection, '0' deactivates it.  A 'switch map' defining these connections exists for each row.
*   **Bus Types:** Multiple bus types are supported:
    *   **Standard Data Bus:** For passing primary input data and partial results.
    *   **Accumulation Bus:**  Dedicated to accumulating partial sums across rows, potentially bypassing certain PEs.
    *   **Broadcast Bus:** Allows a single PE to broadcast a value to multiple PEs within a row.
*   **Dynamic Reconfiguration Frequency:**  Reconfiguration can occur at the beginning of each systolic interval, or at a coarser granularity (e.g., every N intervals) to reduce overhead.
*   **Dataflow Graph Compilation:** A compiler will translate a target kernel into a dataflow graph. This graph specifies the data dependencies and operations required.
*   **Mapping Algorithm:** An algorithm maps the dataflow graph onto the systolic array. This involves:
    *   Assigning operations to PEs.
    *   Determining the optimal bus topology for each row to efficiently route data between PEs.
    *   Generating the configuration data for each row’s controller.

**Pseudocode (Mapping Algorithm - Simplified):**

```
function map_dataflow_graph(graph, array_size):
  // graph: Dataflow graph representing the kernel
  // array_size: Dimensions of the systolic array

  PE_assignment = assign_operations_to_PEs(graph, array_size)
  bus_topology = {} // Dictionary to store bus configurations for each row

  for row in range(array_size[0]):
    bus_topology[row] = {}
    for PE in row:
      bus_topology[row][PE] = [] // List of buses connected to this PE

      // Determine data dependencies for this PE
      input_PEs = find_input_PEs(PE, graph)
      output_PEs = find_output_PEs(PE, graph)

      // Connect to necessary input buses
      for input_PE in input_PEs:
        bus = find_best_bus(input_PE, PE) // Algorithm to select optimal bus
        bus_topology[row][PE].append(bus)

      // Connect to necessary output buses
      for output_PE in output_PEs:
        bus = find_best_bus(PE, output_PE)
        bus_topology[row][PE].append(bus)

  // Generate configuration data for each row based on bus_topology

  return configuration_data
```

**Potential Benefits:**

*   **Kernel Adaptability:** Support for a wider range of kernels without hardware redesign.
*   **Dataflow Optimization:** Optimize dataflow based on input data characteristics.
*   **Fault Tolerance:**  Dynamically re-route data around faulty PEs.
*   **Reduced Hardware Complexity:**  Reduce the need for specialized hardware for different kernels.