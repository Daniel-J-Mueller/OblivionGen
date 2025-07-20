# 10943167

## Dynamic Sparsity via Reconfigurable Interconnect

**Concept:** Implement a hardware architecture that dynamically adjusts the interconnect between processing elements (PEs) to create sparse matrix operations *without* masking or dedicated sparse hardware. The key is a reconfigurable crossbar switch network *within* the array, allowing PEs to selectively ‘mute’ their connections during computation.

**Specs:**

*   **Array Architecture:** Maintain the configurable array of processing elements described in the patent. Each PE retains the arithmetic units for multiplication and accumulation.
*   **Interconnect:** Introduce a NxN crossbar switch network *between* adjacent PEs in both dimensions (input and output). ‘N’ represents the number of PEs in each dimension of the array. This network is composed of programmable switches (e.g., CMOS transmission gates).
*   **Switch Control:** Each switch is individually controllable by the central controller. The controller can set a switch to either ‘pass’ (connected) or ‘block’ (disconnected) a signal.
*   **Sparsity Mapping:** The controller implements a sparsity mapping algorithm. Given a sparse matrix, the algorithm determines which connections *should* be blocked to represent the zero elements.
*   **Data Flow:** Input data is fed into the array as usual. The controller, *before* the multiplication-accumulation cycle, configures the crossbar switches based on the sparsity mapping.  This creates an effective sparse connectivity between PEs.
*   **Computation:**  PEs perform their multiplication-accumulation cycles *only* on connected inputs. Disconnected inputs are effectively ignored. This eliminates unnecessary operations.
*   **Dynamic Reconfiguration:** The sparsity mapping and crossbar configuration can be dynamically changed between layers of a neural network or between successive computations.

**Pseudocode (Controller):**

```
function configure_sparsity(sparse_matrix, array_dimensions):
  // sparse_matrix is a 2D array representing the sparse matrix
  // array_dimensions is a tuple (rows, cols) representing the array size

  for row in range(array_dimensions[0]):
    for col in range(array_dimensions[1]):
      if sparse_matrix[row][col] == 0:
        // Block the connection between PE(row, col) and its neighbors
        set_switch(row, col, neighbor, "BLOCK") //Function to set switch states
      else:
        set_switch(row, col, neighbor, "PASS")
```

**Implementation Details:**

*   The crossbar switches add latency. The switch design must be optimized for minimal delay.
*   The controller requires a significant amount of control logic to manage the switches.
*   Power consumption is a concern. Optimized switch design and selective enabling of switches can mitigate this.
*   The reconfigurability provides flexibility beyond just sparse matrices. It allows for non-standard data flow patterns and custom neural network architectures.