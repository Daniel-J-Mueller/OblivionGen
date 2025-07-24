# 11120006

## Distributed Data Shadowing with Predictive Consistency

**Concept:** Extend the sequence-based ordering from the patent to allow for *shadowing* of data across storage nodes *before* a transaction commits, coupled with predictive consistency levels. This aims to improve read performance and reduce latency for operations that can tolerate eventual consistency.

**Specification:**

**1. Shadow Node Selection:**

*   Each storage node maintains a configurable 'shadow factor' (SF). This factor defines the number of peer storage nodes to which it will replicate data as shadows.
*   Shadow node selection is not static. Nodes dynamically choose shadows based on network proximity (latency), load, and data access patterns. A distributed hash table (DHT) manages shadow node mappings.
*   The transaction coordinator (TC) queries the DHT to identify appropriate shadow nodes *before* sending the initial prepare request.

**2. Predictive Consistency Levels:**

*   Introduce a new dimension to consistency levels beyond standard read/write configurations (strong, eventual, etc.). This new dimension is 'predictive confidence'.
*   Each read request includes a 'tolerance factor' (TF). This TF represents the acceptable probability of reading stale data.
*   Shadow nodes track data 'staleness score' (SS). SS is based on the number of committed transactions *since* the shadow copy was created.
*   Read requests are routed to the shadow node with the lowest SS *that also satisfies the TF*. The SS must be below the TF threshold for the read to proceed.

**3. Transaction Flow Modification:**

*   **Prepare Phase:** The TC sends the prepare request to the primary storage node *and* the selected shadow nodes simultaneously.
*   **Commit Phase:** If all nodes (primary & shadows) acknowledge the prepare request, the TC sends the commit request.  Shadows update their copies *before* the primary confirms the commit.
*   **Rollback Phase:** If any node rejects the prepare request, the TC initiates a rollback. Shadows discard their pre-committed changes.

**4. Data Versioning:**

*   Each data element is tagged with a vector clock. This vector clock tracks the source node and the sequence number of the last write.
*   Shadow nodes compare vector clocks during data replication. If a shadow node has a newer version (higher sequence number), it does *not* overwrite it with an older version. This prevents divergence.

**5. Failure Handling:**

*   If a shadow node fails *during* a transaction, the TC selects a new shadow node and re-sends the prepare request.
*   If the primary node fails *after* shadows have been updated, the TC promotes one of the shadows to become the new primary.

**Pseudocode (Simplified):**

```
// Read Request
function handleReadRequest(dataId, toleranceFactor):
  shadowNodes = getShadowNodes(dataId)
  bestShadowNode = null
  minStalenessScore = infinity

  for node in shadowNodes:
    stalenessScore = node.getStalenessScore(dataId)
    if stalenessScore <= toleranceFactor and stalenessScore < minStalenessScore:
      minStalenessScore = stalenessScore
      bestShadowNode = node

  if bestShadowNode != null:
    return bestShadowNode.readData(dataId)
  else:
    // Fallback to primary node (higher latency)
    return primaryNode.readData(dataId)
```

```
// Commit Phase (Simplified)
function commitTransaction(transactionData):
  primaryNode.commit(transactionData)
  for shadowNode in shadowNodes:
    shadowNode.commit(transactionData)
  return true
```