# 10666775

## Adaptive Packet Shaping & Re-Sequencing for Network Virtualization

**Concept:** Extend the internal packet generation/checking system to dynamically shape and re-sequence packets *before* they reach the egress port, enabling advanced network virtualization features like virtual network function (VNF) chaining and customized traffic flows on a per-subscriber or per-application basis.

**Specs:**

*   **Module:** Add a "Packet Sculptor" module between the packet checker and the egress port. This module will be implemented in hardware within the integrated circuit.
*   **Configuration:** The Packet Sculptor will be configured via the existing configuration register and host interface, but with expanded capabilities.  New registers will be added to define "flow profiles."
*   **Flow Profiles:** A flow profile will contain:
    *   **Match Criteria:**  Fields to match on (e.g., source/destination IP address, port number, VLAN tag, DSCP).
    *   **Shaping Rules:** Instructions for modifying the packet:
        *   **Delay Insertion:** Introduce a variable delay (in microseconds) to the packet.
        *   **Reordering:**  Change the order of packets belonging to a TCP stream (limited to a window size of, say, 10 packets).
        *   **Payload Modification:**  Limited modification of the packet payload (e.g., adding a timestamp, injecting a small control flag).
        *   **DSCP/VLAN Modification:** Alter DSCP values or VLAN tags.
    *   **Action:**  Specify whether to apply these rules.
*   **Packet Processing Flow:**
    1.  Packet arrives from the packet processor.
    2.  Packet checker validates the packet.
    3.  Packet is matched against active flow profiles (using a hardware-accelerated lookup table).
    4.  If a match is found, the shaping rules are applied by the Packet Sculptor.
    5.  Modified packet is transmitted via the egress port.
*   **Internal Mode Enhancement:** The internal mode will be extended to allow the Packet Generator to create a sequence of packets specifically designed to test the Packet Sculptor’s capabilities.  This could involve creating out-of-order packets, packets with invalid checksums (to test error handling), and packets with specific DSCP values.
*   **Hardware Acceleration:**  Critical operations (pattern matching, checksum calculation, delay insertion) must be implemented in hardware to minimize latency. Use a dedicated hardware engine for DSCP and VLAN tag alterations, as this may be a frequently performed operation.
*   **Scalability:** Design the flow profile lookup table to support a large number of profiles (e.g., 1024 or more).  Consider using a hierarchical lookup scheme to improve performance.
*   **Statistics:** Track statistics on the number of packets processed, the number of packets matched against flow profiles, and the amount of delay inserted. These statistics can be used for monitoring and troubleshooting.

**Pseudocode (Flow Profile Matching):**

```
function match_flow_profile(packet, flow_profile_table):
  for profile in flow_profile_table:
    if profile.match(packet):
      return profile
  return null // No matching profile found

function apply_shaping_rules(packet, profile):
  // Apply delay insertion
  packet.delay = profile.delay
  // Reorder packets (if applicable)
  if profile.reorder_enabled:
    // ... reordering logic ...
  // Modify payload (if applicable)
  if profile.payload_modification_enabled:
    // ... payload modification logic ...
  // Modify DSCP/VLAN
  packet.dscp = profile.dscp
  packet.vlan = profile.vlan
  return packet
```