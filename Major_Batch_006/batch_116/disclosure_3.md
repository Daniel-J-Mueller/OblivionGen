# 11824961

## Dynamic Payload Fragmentation for Link Characterization

**Concept:** Instead of sending a single, fixed-size payload to determine TCP throughput, dynamically fragment the payload into variable-sized packets. Analyze the reassembly time and packet loss rates of these fragments to gain a more granular understanding of link characteristics beyond simple throughput – specifically, Minimum Segment Size (MSS) negotiation issues, Path MTU Discovery (PMTUD) failures, and bufferbloat.

**Specifications:**

1.  **Fragmentation Engine:**
    *   Input: Destination IP/MAC, Base Payload Data.
    *   Parameters:
        *   `fragment_count`: Number of fragments to create.
        *   `fragment_size_variation`: Percentage variation allowed in fragment sizes.
        *   `reassembly_timeout`: Timeout value for reassembly attempts.
        *   `initial_fragment_size`: Initial fragment size in bytes.
    *   Process:
        *   Divide the base payload into `fragment_count` segments.
        *   Apply random variation (defined by `fragment_size_variation`) to each fragment's size, ensuring no fragment exceeds the calculated PMTUD.
        *   Add sequence numbers to each fragment for reassembly.
        *   Set the ‘Don’t Fragment’ (DF) bit in the IP header of *all* fragments. This forces ICMP messages upon PMTUD failure.
        *   Construct IP/TCP headers for each fragment, including appropriate flags and offsets.

2.  **Transmission & Reception Protocol:**
    *   Send all fragments consecutively.
    *   Implement a fragment reassembly buffer on the receiving end.
    *   Upon receiving a fragment, store it in the buffer, indexed by its sequence number.
    *   Maintain a timestamp for each received fragment.
    *   Upon receiving the final fragment (identified by a sequence number flag), initiate reassembly.
    *   Implement a timeout mechanism for missing fragments. If a fragment is missing after a certain period, flag the entire transmission as failed.

3.  **Link Characterization Algorithm:**
    *   `Total Transmission Time = Timestamp(last fragment received) - Timestamp(first fragment sent)`
    *   `Average Fragment Size = Total Payload Size / fragment_count`
    *   `Inter-Fragment Delay = Max(Timestamp(fragment_n) - Timestamp(fragment_n-1)) for all n > 1`
    *   `PMTUD Failure Rate = Count of ICMP ‘Fragmentation Needed’ messages received / fragment_count`
    *   `Estimated MSS = Minimum Fragment Size observed (excluding PMTUD failures)`
    *   `Bufferbloat Indicator = Variance of Inter-Fragment Delays`. A high variance suggests significant queueing delays on intermediate devices.

4.  **PMTUD Probe Control:**
    *   Initiate a baseline probe with a small `initial_fragment_size`.
    *   If PMTUD failures occur, decrease `initial_fragment_size` by a percentage.
    *   If no PMTUD failures occur for a defined number of iterations, increase `initial_fragment_size` by a percentage.
    *   This adaptive probing process automatically determines the maximum achievable fragment size without triggering PMTUD failures.

**Pseudocode (Receiver):**

```
fragment_buffer = {}
expected_fragments = fragment_count
last_received_time = current_time()

while (fragments_received < expected_fragments):
    fragment = receive_packet()
    fragment_number = extract_sequence_number(fragment)
    fragment_buffer[fragment_number] = fragment
    last_received_time = current_time()
    fragments_received++

if (fragments_received == expected_fragments):
    reassemble_payload(fragment_buffer)
    calculate_metrics(start_time, last_received_time, fragment_buffer)
else:
    log_error("Fragment Loss: Transmission Failed")
```

**Novelty:** Traditional TCP throughput measurements rely on sending a consistent stream of data. This approach misses nuances related to packet fragmentation, MTU discovery, and bufferbloat. This dynamic fragmentation system provides a far richer dataset, allowing for the identification of subtle network issues that would otherwise remain hidden. It adapts, rather than tests a fixed parameter.