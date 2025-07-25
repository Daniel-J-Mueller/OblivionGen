# 11308026

## Dynamic Reconfiguration of Systolic Array Topology

**Concept:**  The core innovation is to move *beyond* static, row-oriented buses in the systolic array. Instead, implement a dynamically reconfigurable interconnect network *within* the array itself, allowing processing elements (PEs) to establish arbitrary connections with other PEs in real-time, rather than being limited to a fixed row-based communication pattern. This is achieved through a mesh network of micro-switches integrated into each PE.

**Specifications:**

*   **PE Architecture:** Each processing element will incorporate a 4x4 micro-switch.  This switch can route signals to/from any of its four neighbors (North, South, East, West). The switch is implemented using CMOS transmission gates for low latency.  Each PE retains the multiplier/adder functionality described in the original patent.
*   **Interconnect:** The entire array is structured as a 2D mesh. Each PE’s micro-switch connects to the corresponding switches in neighboring PEs, forming the mesh network.
*   **Control Plane:** A dedicated control unit (CU), external to the array but communicating via a high-bandwidth interface, manages the micro-switches. The CU contains a routing table pre-programmed with common systolic algorithms (matrix multiplication, convolution, etc.). It can also accept dynamically generated routing instructions.
*   **Routing Protocol:** The routing protocol operates on a cycle-by-cycle basis. Before each cycle, the CU broadcasts a routing configuration to all micro-switches. The configuration specifies, for each PE, which neighbor’s output should be routed to its input, and which PE's output should be routed to its neighbor’s input.
*   **Data Flow Control:** A token-passing mechanism ensures data integrity and prevents deadlocks. A token circulates through the array, granting a PE permission to transmit data.  The token is passed along the configured data path.

**Pseudocode (Routing Configuration Broadcast):**

```
// CU Routing Configuration Broadcast
function broadcast_configuration(routing_table):
  for each PE in array:
    configuration = routing_table[PE.id]  // PE.id is unique identifier
    send_configuration(PE, configuration)

function send_configuration(PE, configuration):
  // configuration contains:
  //   input_port:  0 (North), 1 (South), 2 (East), 3 (West)
  //   output_port: 0 (North), 1 (South), 2 (East), 3 (West)
  PE.microswitch.set_input_port(configuration.input_port)
  PE.microswitch.set_output_port(configuration.output_port)
```

**Novelty & Potential:**

*   **Algorithm Flexibility:** The system is no longer limited to algorithms well-suited for row-oriented data flow. It can implement a much wider range of computations.
*   **Dynamic Load Balancing:** The routing can be adjusted to distribute workload evenly across the array, improving performance.
*   **Fault Tolerance:** If a PE fails, the routing can be reconfigured to bypass it, maintaining functionality.
*   **Sparse Matrix Acceleration:**  The dynamic routing allows for efficient computation of sparse matrices, where many elements are zero.  Non-zero elements can be routed directly to the relevant PEs, bypassing the zero elements.