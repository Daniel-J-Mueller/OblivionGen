# 10891267

## Adaptive Partition Map Propagation based on Query Load

**Concept:** Dynamically adjust partition map update propagation based on real-time query load. Instead of broadcasting updates or selectively sending to replica groups, prioritize nodes currently handling queries related to the changed partitions. This minimizes disruption and maximizes responsiveness.

**Specifications:**

1.  **Query Load Monitoring:**
    *   Implement a monitoring system that tracks queries accessing specific partitions. This could be done at a proxy layer or within the database nodes themselves.
    *   Metrics: Queries per second, average query latency, total data scanned per partition.
    *   Data Structure: `PartitionQueryLoad` = {partitionID: {qps: float, latency: float, scanned: long}}.

2.  **Partition Change Event:**
    *   When a partition change (split, merge, move, etc.) is detected, create a `PartitionChangeEvent` object.
    *   `PartitionChangeEvent` = {changeType: enum (SPLIT, MERGE, MOVE, etc.), affectedPartitions: list of partitionIDs, newPartitionMap: map of partitionID to node list}.

3.  **Prioritization Algorithm:**
    *   Upon receiving a `PartitionChangeEvent`, use the `PartitionQueryLoad` data to prioritize nodes for update.
    *   Algorithm:
        1.  Identify nodes currently serving queries for the `affectedPartitions`.
        2.  Rank these nodes by their current query load (highest qps first).
        3.  Send the `newPartitionMap` *only* to the top N ranked nodes.  N is a configurable parameter.
        4.  For remaining nodes, use the original propagation strategy (broadcast or replica group).

    ```pseudocode
    function propagatePartitionMap(partitionChangeEvent):
      affectedPartitions = partitionChangeEvent.affectedPartitions
      nodesWithLoad = getNodesWithLoadForPartitions(affectedPartitions) // returns list of nodes sorted by qps

      topN = getConfigValue("topN")
      nodesToUpdate = nodesWithLoad.slice(0, topN)

      remainingNodes = getAllNodes() - nodesToUpdate

      sendPartitionMapToNodes(nodesToUpdate, partitionChangeEvent.newPartitionMap)
      sendPartitionMapToNodes(remainingNodes, partitionChangeEvent.newPartitionMap) // use original strategy
    ```

4.  **Dynamic Adjustment of N:**
    *   Implement a feedback loop to dynamically adjust the value of N based on system performance.
    *   If query latency increases after an update, increase N.
    *   If resources are constrained, decrease N.

5.  **Stale Partition Map Detection:**
    *   Implement a mechanism to detect nodes with stale partition maps.
    *   Nodes periodically report their partition map version.
    *   If a discrepancy is detected, trigger a targeted update.

6.  **Integration with Existing Partition Map Propagation:**
    *   This system should be implemented as a layer on top of the existing partition map propagation mechanism.
    *   It should not require major changes to the core database architecture.