# 10103962

## Adaptive Packet Sculpting for Network Topology Discovery

**Concept:** Leveraging the time-to-live (TTL) mechanism, not just for tracing, but for *actively shaping* packets to map network topology in real-time, bypassing reliance on static routing tables or broadcast/multicast mechanisms.  This aims for a dynamic, granular understanding of network connections and potential bottlenecks, focusing on latency *variation* rather than simple round trip time.

**Specs:**

*   **Core Component:** ‘Topology Probe’ – a software module embedded within network devices (routers, switches, end-hosts).
*   **Packet Structure:**  Modified ICMP/TCP packets containing:
    *   Standard headers.
    *   ‘Sculpt ID’ – Unique identifier for a probe sequence.
    *   ‘Sculpt Value’ –  An integer value initially randomized and progressively modified.
    *   ‘Hop Count’ - Counts the number of hops.
    *   ‘Timestamp’ – High-resolution timestamp at each hop.
*   **Probe Initiation:** Topology Probe periodically initiates ‘Sculpted Probes’.
*   **Sculpting Algorithm:**  At each hop:
    1.  Read ‘Sculpt Value’.
    2.  Apply a pseudorandom function to the ‘Sculpt Value’ based on hop-specific information (e.g., interface index, AS number, device ID).  The function should be deterministic given the same inputs.
    3.  Modify the ‘Sculpt Value’ with the result.
    4.  Append timestamp and forward.
*   **Return Path Analysis:**  The source (Topology Probe initiator) receives the sculpted probes.  Analysis focuses on:
    1.  **Sculpt Value Divergence:**  Comparison of the returned ‘Sculpt Value’ with the initial value.  High divergence indicates alternative paths.
    2.  **Timestamp Variance:**  Examination of timestamp differences between hops.  Large variance suggests congestion or latency fluctuations.
    3.  **Path Reconstruction:**  Based on timestamp, hop count, and ‘Sculpt Value’ the source reconstructs the actual path taken.
*   **Dynamic Topology Map:**  The system builds a dynamic map reflecting real-time network conditions. The map isn’t static; it adjusts to changing conditions.
*   **Adaptive Probing:** Probe rate and ‘Sculpt Value’ modification parameters are dynamically adjusted based on network load and observed conditions.

**Pseudocode (Topology Probe – Sending side):**

```
function initiateProbe(destinationAddress):
  sculptID = generateUniqueID()
  initialSculptValue = randomInteger(1, 1000)
  packet = createICMPPacket()
  packet.destinationAddress = destinationAddress
  packet.sculptID = sculptID
  packet.sculptValue = initialSculptValue
  sendPacket(packet)

function processReturnedPacket(packet):
  if packet.sculptID == mySculptID:
    calculatePathDivergence(packet.sculptValue)
    updateTopologyMap(packet.pathData)
```

**Pseudocode (Network Device - Intermediate):**

```
function receivePacket(packet):
  if packet is Sculpted Probe:
    newSculptValue = applySculptingFunction(packet.sculptValue, deviceID, interfaceIndex)
    packet.sculptValue = newSculptValue
    addTimestamp(packet)
    forwardPacket(packet)
  else:
    forwardPacket(packet)
```

**Innovation Focus:**  The 'Sculpting Function' is the key.  It goes beyond simple TTL decrementing, actively shaping the probe packet’s data to create a unique ‘fingerprint’ of each path. This enables significantly more granular path identification and congestion detection than traditional tracing methods. It's less about *finding* a path, and more about *characterizing* the path’s properties.