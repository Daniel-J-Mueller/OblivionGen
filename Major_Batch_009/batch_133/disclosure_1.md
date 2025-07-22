# 11288004

## Adaptive Replica Prioritization & Tiering

**Concept:** Extend the heartbeat/failure detection mechanism to incorporate dynamic replica prioritization *and* tiering based on observed performance characteristics, workload type, and cost. Instead of simply removing a failed/unresponsive replica, the system dynamically adjusts replica roles based on continuous monitoring.

**Specifications:**

**1. Performance Monitoring Module:**

*   **Metrics:** Each replica continuously reports (via heartbeat extended data) the following:
    *   IOPS (Input/Output Operations Per Second)
    *   Latency (Average & 99th percentile)
    *   CPU Utilization
    *   Network Bandwidth Used
    *   Storage Queue Length
    *   Workload Type (Identified through I/O pattern analysis – e.g., sequential read, random write, mixed)
*   **Aggregation:** A central “Replica Performance Manager” (RPM) aggregates these metrics for all replicas.
*   **Baseline Calculation:**  RPM maintains a rolling baseline for each replica’s performance under typical load.
*   **Anomaly Detection:** RPM identifies performance anomalies (significant deviations from the baseline) for each replica.

**2. Tiering & Prioritization Logic:**

*   **Tiers:** Define tiers based on performance characteristics and cost:
    *   **Tier 0: “Hot”**:  Lowest latency, highest IOPS, most expensive storage (e.g., NVMe SSDs).  Primary read/write operations.
    *   **Tier 1: “Warm”**: Medium latency, medium IOPS, moderate cost (e.g., SATA SSDs). Read cache, background writes.
    *   **Tier 2: “Cold”**: Higher latency, lower IOPS, lowest cost (e.g., HDDs). Archive, infrequently accessed data.
*   **Prioritization:** Within each tier, replicas are ranked based on a weighted score calculated from:
    *   Performance Metrics (IOPS, Latency) – 60%
    *   Workload Alignment (How well the replica handles the current workload type) – 20%
    *   Cost (Lower cost = Higher score) – 20%

**3. Dynamic Role Adjustment:**

*   **Heartbeat Extension:** Heartbeat messages now include replica performance scores *and* current tier assignments.
*   **Failure Detection Enhancement:** When a replica fails heartbeat *or* its performance score drops below a threshold:
    *   The system *does not immediately remove* the replica.
    *   The RPM re-evaluates the remaining replicas.
    *   It dynamically promotes lower-tier replicas to higher tiers, assigning them primary roles if necessary.
    *   The failed replica is demoted to the lowest possible tier (potentially offline for repair) *without* disrupting ongoing operations.
*   **Workload-Aware Routing:** The system directs I/O requests to the highest-priority replica *capable* of handling the specific workload type. This ensures optimal performance and resource utilization.

**4. Pseudocode – Dynamic Promotion/Demotion**

```
function handleReplicaFailure(failedReplica) {
  // Get list of active replicas
  activeReplicas = getActiveReplicas()

  // Calculate priority scores for each active replica
  for each replica in activeReplicas {
    replica.priorityScore = calculatePriorityScore(replica)
  }

  // Sort replicas by priority score (descending)
  sortedReplicas = sort(sortedReplicas, descending)

  // Promote lower-tier replicas to fill the gap
  for (i = 0; i < sortedReplicas.length; i++) {
    if (sortedReplicas[i].tier < currentTier) { //If replica is a lower tier than the tier that has had a failure
      sortedReplicas[i].promoteTier() // Promote the tier
      break // Only promote one replica at a time
    }
  }
  // Demote failed replica to Tier 2 (cold storage) or offline for repair.
  failedReplica.demoteTier(2) // Demote failed replica to cold storage
}
```

**5.  Generation Number Integration:**

*   During tier promotion/demotion, replicas verify the generation number of the RPM before accepting new roles. This prevents split-brain scenarios or conflicting configuration updates.

**Potential Benefits:**

*   Improved system resilience.
*   Optimized performance and resource utilization.
*   Reduced operational costs.
*   Adaptive to changing workloads.
*   Enhanced data availability.