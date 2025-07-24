# 9367252

## Adaptive Quorum Based on Replica Latency

**Specification:** A system component monitoring inter-replica communication latency, dynamically adjusting quorum size and composition to prioritize low-latency replicas during master election/failover.

**Rationale:** The provided patent details a system for data replication with a fixed quorum size. Real-world network conditions fluctuate.  A fixed quorum may include slower, less reliable replicas, unnecessarily delaying failover or impacting write consistency. Adapting the quorum to favor fast replicas can significantly improve performance and resilience.

**Components:**

1.  **Latency Monitor:**  A background process on each node measuring round-trip time (RTT) to all other replicas in the group.  Utilizes periodic pings or incorporation into regular heartbeat messages. Stores historical latency data (e.g., moving average over last 5 minutes) for each replica.

2.  **Quorum Manager:** Responsible for initiating and managing master election/failover.  Interacts with the Latency Monitor to determine current replica latency.

3.  **Dynamic Quorum Algorithm:**

    *   **Initial Quorum:** Defined statically (as in the provided patent).
    *   **Latency Ranking:** The Quorum Manager requests the latest latency rankings from the Latency Monitor.  Replicas are sorted by average latency (lowest to highest).
    *   **Quorum Adjustment:**
        *   If the current master fails, the Quorum Manager calculates a dynamic quorum.
        *   The top *N* replicas (based on latency ranking) are *always* included in the dynamic quorum. *N* is configurable (e.g., majority + 1, or a fixed number like 3).
        *   If fewer than the required number of replicas are available (due to failure or network issues), the algorithm gracefully degrades, potentially reducing the quorum size and issuing warnings.
        *   Replica weighting may be introduced – replicas with very low latency have greater 'voting power' in the quorum.
    *   **Failover Procedure:** The failover procedure is initiated only when the dynamic quorum is established.
    *   **Continuous Monitoring:** The Latency Monitor continues to track latency, and the dynamic quorum is recalculated upon each failover attempt.
4.  **Configuration Parameters:**

    *   `quorum.initial_size`: The initial, static quorum size.
    *   `quorum.dynamic_enabled`: Boolean flag to enable/disable dynamic quorum.
    *   `quorum.top_n`: Number of top-performing replicas always included in the dynamic quorum.
    *   `quorum.latency_threshold`:  Latency value (in milliseconds) above which a replica is considered 'slow'.
    *    `quorum.recalculation_interval`: Frequency with which the quorum is recalculated.

**Pseudocode (Quorum Manager):**

```
function initiateFailover():
  if dynamicQuorumEnabled:
    dynamicQuorum = calculateDynamicQuorum()
    if size(dynamicQuorum) >= requiredQuorumSize:
      proceedWithFailover(dynamicQuorum)
    else:
      logWarning("Insufficient replicas for quorum. Failover aborted.")
  else:
    proceedWithFailover(initialQuorum)

function calculateDynamicQuorum():
  latencyRankings = LatencyMonitor.getLatencyRankings()
  topNReplicas = latencyRankings.slice(0, quorum.top_n)
  dynamicQuorum = topNReplicas
  // Add additional replicas (if needed) to meet quorum size. Prioritize next lowest latency.
  while size(dynamicQuorum) < requiredQuorumSize:
    nextReplica = findNextLowestLatencyReplica(latencyRankings, dynamicQuorum)
    dynamicQuorum.add(nextReplica)
  return dynamicQuorum

function findNextLowestLatencyReplica(latencyRankings, existingQuorum):
  for replica in latencyRankings:
    if not replica in existingQuorum:
      return replica
  return null // or handle the case where no more replicas are available.
```

**Potential Benefits:**

*   Reduced failover latency.
*   Improved write consistency under variable network conditions.
*   Increased resilience to slow or failing replicas.
*   Adaptability to dynamic network topologies.