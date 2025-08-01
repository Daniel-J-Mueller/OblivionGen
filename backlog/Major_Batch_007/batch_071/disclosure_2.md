# 11301144

## Adaptive Data Locality with Predictive Prefetching

**Concept:** Enhance data access speeds and reduce latency by predicting data access patterns and proactively positioning data closer to compute nodes *before* requests arrive. This moves beyond simple replication and erasure coding to a dynamic, predictive system.

**Specifications:**

**1. Component: ‘Locality Agent’**
    *   **Location:** Deployed on each head node.
    *   **Function:** Monitors I/O requests for volume partitions, builds historical access profiles (time-series data of block/object access), and implements a predictive model.
    *   **Predictive Model:** A recurrent neural network (RNN) – specifically, a Long Short-Term Memory (LSTM) network – trained on historical access patterns. Input: sequence of accessed block/object IDs. Output: probability distribution of the next block/object ID to be accessed.
    *   **Data Structures:**
        *   `AccessHistory`: A circular buffer storing recent block/object access timestamps and IDs.
        *   `PredictionModel`: The trained LSTM network.
        *   `LocalityScore`: A weighted score reflecting the predicted probability of access, frequency of access, and distance from the current compute node.

**2. Component: ‘Data Placement Engine’**
    *   **Location:** Centralized control plane component.
    *   **Function:** Receives `LocalityScore` updates from all Locality Agents. Makes data placement decisions based on these scores, dynamically moving data between mass storage devices and head node caches.
    *   **Algorithm:**
        1.  Receive `LocalityScore` from Locality Agents.
        2.  Identify blocks/objects with the highest `LocalityScore`.
        3.  Determine the optimal destination for these blocks/objects:
            *   **Tier 1: Head Node Cache:** For extremely high scores, load into the head node’s local cache.
            *   **Tier 2: Adjacent Mass Storage:** Move to a mass storage device physically close to the anticipated compute node.
            *   **Tier 3: Remote Mass Storage:** Maintain on existing remote storage.
        4.  Initiate data transfer.

**3. Component: ‘Prefetch Controller’**
    *   **Location:** Within each head node.
    *   **Function:** Based on the `PredictionModel`, proactively fetches data *before* a request is received.
    *   **Algorithm:**
        1.  Retrieve the top N predicted block/object IDs from the `PredictionModel`.
        2.  Check if these blocks/objects are already in the local cache.
        3.  If not, initiate a prefetch request to the Data Placement Engine.

**Pseudocode (Prefetch Controller):**

```
function prefetchData():
  predictedBlocks = PredictionModel.predictNextBlocks(N)
  for block in predictedBlocks:
    if not block in localCache:
      request = DataPlacementEngine.requestBlock(block)
      localCache.store(request.data, block)
```

**4. Data Structures:**

*   **`LocalityScore`:**  Float value representing the combined prediction, access frequency, and proximity.
*   **`AccessProfile`**: Time-series data of block/object access, used for training the `PredictionModel`.
*   **`PredictionModel`**: Trained LSTM network, stored and updated by the Data Placement Engine.

**Novelty:**

This system goes beyond replication and erasure coding by *predicting* data access patterns and proactively positioning data closer to compute resources. The use of LSTM networks allows the system to learn complex temporal relationships in data access, improving prediction accuracy and reducing latency. The tiered data placement strategy optimizes data locality and minimizes network bandwidth consumption.