# 9553809

## Dynamic Packet Payload Splitting & Reassembly

**Concept:** Extend the distributed load balancing system to intelligently split large packet payloads across multiple server nodes for parallel processing, then reassemble the results before transmission to the client. This is aimed at accelerating computationally intensive requests.

**Specs:**

*   **Component:** *Payload Splitter/Reassembler Module (PSRM)* – integrated into each server node.
*   **Trigger:** Ingress server identifies packets exceeding a configurable size threshold *and* marked with a specific flag (e.g., a DSCP value).
*   **Splitting Logic:** PSRM divides the payload into *n* equal-sized chunks. Chunk size dynamically adjusted based on server node load and network conditions.
*   **Routing:**  Ingress server distributes each chunk to a *different* server node.  Routing considers current server load using a real-time distributed hash table (DHT) maintained by the load balancer nodes.
*   **Processing:** Each server node independently processes its assigned payload chunk.
*   **Reassembly:** A designated "Reassembly Node" (determined by consistent hashing based on client IP/port) collects processed chunks.  Chunks are reordered (sequence numbers embedded in chunk headers) and reassembled into the complete result.
*   **Transmission:** Reassembled result sent back through the egress server to the client.
*   **Error Handling:**  If a server node fails during processing, the Reassembly Node initiates a re-request of the failed chunk from another node (redundancy via chunk replication). Timeouts are implemented to prevent indefinite waiting.
*   **Flow Control:** Reassembly Node dynamically adjusts the chunk size and number of parallel requests to avoid overwhelming server nodes or the network.

**Pseudocode (Reassembly Node):**

```
function reassemblePacket(packetFlowID, totalChunks, chunkSequenceNumbers) {
    receivedChunks = {}
    while (length(receivedChunks) < totalChunks) {
        chunk = receiveChunk()
        if (chunk.sequenceNumber in chunkSequenceNumbers && chunkSequenceNumbers[chunk.sequenceNumber] == false) {
          chunkSequenceNumbers[chunk.sequenceNumber] = true
          receivedChunks[chunk.sequenceNumber] = chunk.payload
        }
    }

    //Sort chunks by sequence number
    sortedChunks = sort(receivedChunks.values(), by sequence number)

    reassembledPayload = concatenate(sortedChunks)

    return reassembledPayload
}

function receiveChunk() {
    //Receive chunk from server node.  Include retry logic with timeouts.
    //Verify checksum/hash for data integrity.
    chunk = receiveData()
    if (verifyChecksum(chunk) == false) {
      //request chunk from another node
    }
    return chunk
}
```

**Configuration Parameters:**

*   `max_packet_size`:  Payload size above which splitting is triggered.
*   `num_chunks`: Default number of chunks to split a payload into.
*   `replication_factor`: Number of redundant copies of each chunk sent to different nodes.
*   `retry_timeout`: Timeout duration for re-requesting failed chunks.
*   `dht_update_interval`: Frequency of updates to the distributed hash table.
*   `chunk_checksum_algorithm`: Algorithm used for chunk checksums (e.g., SHA256).
*   `priority_queue_weight`: Weight applied to reassembly jobs in the priority queue.
*   `priority_queue_size`: Max size of the priority queue.