# 10635650

## Adaptive Granularity Indexing

**Concept:** Extend the auto-partitioning approach to dynamically adjust partition *size* based on query patterns and data distribution *within* a partition, not just the number of partitions. This moves beyond simple horizontal partitioning to a more nuanced approach mimicking hierarchical data structures.

**Specification:**

1.  **Tiered Partitioning:** Each primary partition (created as in the base patent) will be subdivided into 'micro-partitions'. These micro-partitions are logical groupings *within* the physical partition and are dynamically managed.

2.  **Query Pattern Monitoring:** A query analysis module will monitor incoming queries for the APSI. It will identify frequently accessed ranges of the sort key *within* each primary partition.

3.  **Micro-Partition Creation/Merging:**
    *   If a range of the sort key is frequently queried *within* a primary partition, a new micro-partition is created to isolate that range.  The range is determined by a configurable sensitivity threshold - how often a range must be hit before creating a micro-partition.
    *   If a micro-partition receives low query traffic for a defined period, it's merged back into its parent primary partition.
    *   Micro-partition size is not fixed. It is dynamically determined by the volume of data covered by the frequently accessed range.

4.  **Query Routing:**
    *   When a query arrives, the system first determines the relevant primary partition.
    *   Within that primary partition, the query is routed *only* to the micro-partitions covering the relevant range of the sort key.
    *   This drastically reduces the amount of data scanned per query.

5.  **Data Storage:** Data is stored within micro-partitions using a compact, sorted structure (e.g., a sorted run-length encoding or similar).

6.  **Repartitioning Integration:** When the system detects a need to repartition the APSI at the primary level (as described in the original patent), the micro-partition structure is preserved during the data migration.

**Pseudocode (Query Routing):**

```
function routeQuery(query, apsi) {
  primaryPartition = determinePrimaryPartition(query, apsi);
  microPartitions = getRelevantMicroPartitions(query, primaryPartition);

  results = [];
  for each microPartition in microPartitions {
    result = queryMicroPartition(query, microPartition);
    results.append(result);
  }
  return aggregateResults(results);
}

function getRelevantMicroPartitions(query, primaryPartition) {
  relevantMicroPartitions = [];
  for each microPartition in primaryPartition.microPartitions {
    if (microPartition.range overlaps query.range) {
      relevantMicroPartitions.append(microPartition);
    }
  }
  return relevantMicroPartitions;
}

```

**Implementation Notes:**

*   Requires a metadata layer to track micro-partition ranges and their association with primary partitions.
*   Performance will be heavily dependent on the efficiency of the micro-partition range overlap detection.
*   Configuration options for sensitivity thresholds, minimum/maximum micro-partition size, and merging/splitting policies.
*   Consider using bloom filters to quickly determine if a micro-partition *definitely* doesn't contain relevant data.