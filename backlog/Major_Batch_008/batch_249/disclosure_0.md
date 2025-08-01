# 11550505

## Dynamic Shard Affinity Based on Data Locality

**Concept:** Extend the virtual shard system to not only adjust *quantity* of shards, but also *affinity* – which physical node/core a virtual shard predominantly executes on – based on observed data locality patterns. This aims to reduce inter-node communication and improve overall processing speed.

**Specifications:**

1.  **Data Locality Profiler:** Implement a profiling component that monitors the key values (as described in claim 12) associated with incoming records. This profiler calculates a 'locality score' for each key value, representing how frequently that key value’s records are processed in close succession. 

    *   The locality score is a decaying average. Recent co-location of records with the same key value contributes more heavily to the score.
    *   The scoring window is configurable (e.g., last 5 seconds, 10 seconds).

2.  **Shard Affinity Map:**  Maintain a mapping between key values and physical nodes/cores. This map initially distributes key values randomly across available resources.

3.  **Dynamic Affinity Adjustment:**

    *   When a record arrives, its key value is checked against the Shard Affinity Map.
    *   If the key value *is* mapped, the record is routed to the corresponding physical node/core for processing by the appropriate virtual shard.
    *   If the key value *is not* mapped, or the locality score exceeds a threshold:
        *   The system identifies an underutilized physical node/core.
        *   A virtual shard is 'pinned' to that node/core.
        *   The key value is added to the Shard Affinity Map, pointing to the pinned shard’s location.
        *   Subsequent records with that key value are routed directly to that location.
    *   If a node/core becomes overloaded:
        *   The system identifies key values mapped to that node with low locality scores.
        *   The mapping for those key values is moved to an underutilized node/core.

4.  **Shard Migration:** Implement a mechanism for migrating virtual shards between physical nodes/cores without interrupting processing.

    *   Use a checkpointing strategy to save the shard's state.
    *   Transfer the state to the target node/core.
    *   Resume processing on the new node.

5.  **Adaptive Checkpointing:** Adjust the frequency of checkpointing based on the volatility of the data and the cost of migration.

**Pseudocode:**

```
//On record arrival:
record = incomingRecord()
keyValue = record.key
if (shardAffinityMap.containsKey(keyValue)):
    physicalNode = shardAffinityMap.get(keyValue)
    routeRecordToNode(record, physicalNode)
else:
    localityScore = calculateLocalityScore(keyValue)
    if (localityScore > threshold):
        availableNode = findAvailableNode()
        pinShardToNode(availableNode)
        shardAffinityMap.put(keyValue, availableNode)
        routeRecordToNode(record, availableNode)
    else:
        routeRecordToRandomNode(record)
```

**Hardware Considerations:**

*   High-speed interconnect between nodes.
*   Sufficient memory on each node to accommodate multiple virtual shards.
*   Hardware support for virtualization and resource isolation.