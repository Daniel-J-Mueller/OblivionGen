# 11182666

## Adaptive Resonance Network with Spatiotemporal Lookup Tables

**Concept:** Implement a fully parallel Adaptive Resonance Theory (ART) network utilizing 3D lookup tables that incorporate both weight values *and* temporal information. This moves beyond static weight storage and allows the network to learn and adapt to time-varying inputs in a massively parallel fashion.

**Specs:**

*   **Network Architecture:** A fully parallel ART architecture composed of FPGAs. Each node represents a category and contains a dedicated 3D lookup table.
*   **3D Lookup Table:**  Each table is organized as follows:
    *   X-axis: Input Vector Elements (normalized)
    *   Y-axis: Weight Vector Elements (pre-programmed/learned)
    *   Z-axis: Time Step.  This stores a *history* of resonance values, allowing the network to detect patterns evolving over time.
    *   Value: Resonance Strength.  A floating-point value representing the degree of match between input and weight vectors at a given time step.
*   **Input Processing:**
    1.  Input vector is normalized.
    2.  Input vector elements address the X-axis of each node’s lookup table.
    3.  Weight vector elements address the Y-axis.
    4.  The current time step addresses the Z-axis.
    5.  The intersection of X, Y, and Z yields a resonance strength.
*   **Resonance Calculation:**
    1.  Each node calculates its resonance strength in parallel.
    2.  A global maximum selection circuit identifies the node with the highest resonance.
    3.  If the maximum resonance exceeds a vigilance threshold, the input is categorized as belonging to that node.
    4.  If no node exceeds the threshold, a new node is created, and its weight vector is initialized based on the input vector.
*   **Weight Update:**
    1.  After categorization, the winning node's weight vector is updated based on the input vector using a learning rate. The learning rate can be time-dependent, allowing for faster learning during initial stages and finer adjustments later.
    2.  Temporal smoothing is applied to the weight vector to reduce noise and improve stability.  This involves averaging the current weight vector with its previous values.
*   **FPGA Implementation Details:**
    *   Each node’s 3D lookup table is implemented using Block RAM on the FPGA.
    *   Parallel processing is achieved by instantiating multiple nodes on the FPGA.
    *   High-speed communication between nodes is implemented using dedicated communication channels on the FPGA.
*   **Pseudocode (Node Update):**

```
function UpdateNode(inputVector, weightVector, timeStep, learningRate, vigilanceThreshold):
  // Normalize inputVector
  normalizedInputVector = Normalize(inputVector)

  // Lookup Resonance Strength
  resonanceStrength = LookupTable(normalizedInputVector, weightVector, timeStep)

  // Check Vigilance
  if resonanceStrength > vigilanceThreshold:
    // Categorize Input
    return True // Input belongs to this category

  else:
    // Create New Category (if applicable)
    // Initialize weightVector based on inputVector
    return False // Input does not belong to this category
```

*   **Expansion:** Create a layered network of these ART nodes, where the output of one layer serves as the input to the next. This allows for hierarchical feature extraction and complex pattern recognition.
*   **Applications:** Time series analysis, anomaly detection, robotics, autonomous driving. The spatiotemporal lookup tables could enable the system to predict future states based on past observations.