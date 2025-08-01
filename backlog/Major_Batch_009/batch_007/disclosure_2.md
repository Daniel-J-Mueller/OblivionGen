# 8583913

## Adaptive Payload Fragmentation & Reassembly for Dynamic Network Conditions

**Concept:** Extend the core idea of probing network connectivity with payloads, but introduce dynamic payload fragmentation and reassembly based on real-time network condition analysis *during* the probe. This isn’t simply sending a chunked payload; it’s actively *adapting* chunk size and reassembly strategy mid-flight.

**Specification:**

**1. System Components:**

*   **Control Plane:** Manages probe initiation, target host lists, and high-level policy (e.g., frequency, probing type).
*   **Fragmentation/Reassembly Engine (FRE):** A module residing on the “trusted computer” (as described in the patent) responsible for splitting payloads into fragments, assigning sequence numbers, and reassembling them. This is *not* static; it adapts.
*   **Network Condition Analyzer (NCA):** Collects real-time network data (latency, packet loss, bandwidth) *during* the probe via ICMP probes, TCP SYN/ACK timing analysis, or other standard methods. This operates on the "trusted computer".
*   **Untrusted System Agent:** Minimal code on the "untrusted computing system" – simply forwards fragments to the trusted computer.
*   **Target Host:** The internal host being probed.

**2. Operation:**

1.  **Probe Initiation:** Control Plane selects a target host and initializes a probe.
2.  **Initial Fragmentation:** FRE splits the payload into an initial set of fragments. Initial fragment size is based on a conservative estimate of network capacity (e.g., a small size).
3.  **Fragment Transmission:** Fragments are sent through the Untrusted System Agent to the Target Host.
4.  **Real-time Analysis:** NCA continuously monitors network conditions *as fragments are in transit*.
5.  **Dynamic Adjustment:**
    *   If NCA detects *improving* conditions (lower latency, less loss), FRE dynamically increases fragment size for subsequent fragments.
    *   If NCA detects *deteriorating* conditions, FRE reduces fragment size and/or increases redundancy (e.g., sending duplicate fragments). It may also switch to a more robust reassembly protocol.
    *   FRE flags fragments with metadata detailing the network conditions *at the time of fragmentation*. This information is used during reassembly.
6.  **Reassembly:** The Target Host receives fragments. Reassembly prioritizes fragments based on metadata (e.g., fragments created during periods of good network conditions are prioritized). A sliding window reassembly algorithm is employed.
7.  **Connectivity Determination:** Successful reassembly and payload verification indicate connectivity. The time taken for reassembly, along with the network condition data, provides a more detailed assessment of network health.
8.  **Reporting:** Results are reported to the Control Plane.

**3. Pseudocode (FRE - Dynamic Adjustment):**

```
function adjustFragmentSize(networkConditionData, currentFragmentSize):
    if networkConditionData.latency < threshold_low AND networkConditionData.packetLoss < threshold_low:
        newFragmentSize = currentFragmentSize * 1.5  // Increase size
    elif networkConditionData.latency > threshold_high OR networkConditionData.packetLoss > threshold_high:
        newFragmentSize = currentFragmentSize * 0.5  // Decrease size
    else:
        newFragmentSize = currentFragmentSize // No change

    if newFragmentSize > max_fragment_size:
        newFragmentSize = max_fragment_size
    if newFragmentSize < min_fragment_size:
        newFragmentSize = min_fragment_size
    return newFragmentSize
```

**4. Data Structures:**

*   **FragmentHeader:**
    *   SequenceNumber (INT)
    *   Timestamp (TIMESTAMP)
    *   FragmentSize (INT)
    *   NetworkConditionData (JSON) – Latency, Packet Loss, Bandwidth at fragmentation time
    *   Checksum (INT)
*   **NetworkConditionData:**
    *   Latency (FLOAT) – milliseconds
    *   PacketLoss (FLOAT) – percentage
    *   Bandwidth (FLOAT) – Mbps

**5. Enhancements:**

*   **Multipath Probing:** Send fragments through multiple paths (if available) to assess network diversity.
*   **AI-Powered Adaptation:** Use machine learning to predict network conditions and optimize fragmentation strategy in real-time.
*   **Proactive Fragmentation:** Analyze historical network data to pre-fragment payloads based on anticipated conditions.