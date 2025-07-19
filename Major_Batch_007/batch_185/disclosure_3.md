# 11310149

## Adaptive Packet Sharding & Reassembly for Multi-Path Resilience

**Concept:** Enhance network resilience and throughput by dynamically sharding packets across multiple network paths, and reassembling them at the destination. This builds on the idea of direction-agnostic routing by extending it to *packet-level* routing decisions. 

**Specification:**

**1. Components:**

*   **Shard Engine (SE):** Resides on the source network device. Responsible for packet sharding and initial path selection.
*   **Path Monitor (PM):** Continuously monitors the health and performance of available network paths (latency, packet loss, bandwidth). This operates independently of the Shard Engine.
*   **Reassembly Engine (RE):** Resides on the destination network device.  Responsible for receiving shards, reordering them, and reconstructing the original packet.
*   **Global Path Table (GPT):** A distributed data structure accessible to both Shard and Reassembly Engines. Contains path health data, capacity, and current shard assignments.

**2. Operation:**

*   **Initial Shard Assignment:** Upon receiving a packet, the SE queries the GPT for available paths. The GPT returns a list of viable paths, ranked by performance. The SE divides the packet into a predetermined number of shards (e.g., 4, 8, 16 – configurable per application or network condition). Each shard is assigned to a different path based on the GPT’s ranking and path capacity.
*   **Shard Header:** Each shard is encapsulated with a header containing:
    *   Shard ID (unique identifier for the shard).
    *   Total Shard Count (number of shards the original packet was divided into).
    *   Original Packet Length.
    *   Checksum (for data integrity verification).
    *   Destination Address.
    *   Source Address.
*   **Path Monitoring & Dynamic Re-Routing:** The Path Monitor continuously updates path health data in the GPT. If a path degrades or fails, the GPT automatically triggers a re-routing process.  New shards (or re-transmitted shards) are assigned to alternative paths. The Shard Engine and Reassembly Engine need to be informed of the updates.
*   **Reassembly:** The Reassembly Engine receives shards on different paths. It uses the shard ID and total shard count to reorder the shards and reconstruct the original packet. The checksum is used to verify data integrity. Shards are temporarily stored in memory.
*   **Flow Awareness:**  The system must track flows and ensure shards belonging to the same flow are consistently routed. This is achieved through the use of flow identifiers (e.g., 5-tuple) included in the shard headers.

**3. Pseudocode (Shard Engine):**

```pseudocode
function shardPacket(packet, flowId):
  paths = getAvailablePathsFromGPT() // Returns ranked list of paths
  shardCount = determineShardCount(packet.size) // Configurable
  shardSize = packet.size / shardCount

  shards = []
  for i = 0 to shardCount - 1:
    shard = extractShard(packet, i * shardSize, shardSize)
    shardHeader = createShardHeader(i, shardCount, packet.size, packet.checksum, packet.destination, packet.source)
    shardWithHeader = encapsulate(shard, shardHeader)
    
    path = selectPath(paths, i) // Select path based on ranking
    sendShard(shardWithHeader, path)

    shards.append(shardWithHeader)
  
  return shards
```

**4.  Considerations:**

*   **Overhead:** The added headers and reassembly process introduce overhead.  The shard count and header size need to be carefully optimized.
*   **Out-of-Order Delivery:** Network paths may deliver shards out of order. The Reassembly Engine needs to handle this gracefully.
*   **Packet Loss:** Packet loss on one path doesn’t necessarily affect the entire packet. The Reassembly Engine needs to request retransmission of lost shards.
*   **Security:**  Shard headers should be cryptographically signed to prevent tampering.
*   **Integration with Existing Routing Protocols:** The system should be compatible with existing routing protocols (e.g., BGP, OSPF).