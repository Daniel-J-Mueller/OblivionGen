# 10257167

## Dynamic VPN Session 'Splitting' & Re-Assembly

**Concept:** Extend the idea of distributing VPN session data across multiple appliances to enable *dynamic* session splitting and re-assembly *during* an active connection. Instead of just failing over or load balancing, actively divide a single user’s traffic across multiple VPN tunnels *simultaneously*, and re-assemble it at a designated egress point.

**Specifications:**

**1. Session Fragmenter Module (Client-Side):**

*   **Function:**  Resides within the VPN client.  Intercepts outbound TCP/UDP packets.
*   **Fragmentation Logic:**  Based on configurable parameters (packet size, flow ID, destination port, application type).  Can operate in several modes:
    *   *Round Robin:* Distributes packets sequentially across available tunnels.
    *   *Hash-Based:*  Uses a hash function (incorporating flow ID & potentially other parameters) to determine tunnel assignment, ensuring packets belonging to the same flow tend to stay on the same tunnel.
    *   *Application-Aware:*  Routes specific application traffic (e.g., VoIP) over tunnels with lower latency or jitter.
*   **Packet Tagging:**  Adds a header tag to each fragmented packet containing:
    *   Fragment ID (sequential number within the flow)
    *   Total Fragments (for the flow)
    *   Tunnel ID (identifying the used VPN tunnel)
    *   Checksum (for integrity)

**2. Distributed Re-Assembler (Egress Point):**

*   **Location:** Can be a dedicated appliance, or integrated into one of the VPN appliances.
*   **Function:** Receives fragmented packets from all VPN tunnels.
*   **Re-Assembly Logic:** 
    *   Uses Fragment ID and Total Fragments to reconstruct the original packets.
    *   Verifies Checksum for data integrity.
    *   Handles out-of-order packet arrival.
*   **Traffic Prioritization:**  Allows prioritization of re-assembled traffic based on application type or user profile.

**3. Control Plane & Metadata Management:**

*   **Session Table:**  Maintained by a central controller. Contains metadata for each active session, including:
    *   Session ID
    *   List of active tunnels
    *   Fragmentation parameters
    *   Egress point address
*   **Tunnel Discovery:**  Dynamic discovery of available VPN tunnels and their performance characteristics.
*   **Dynamic Adjustment:** Ability to dynamically adjust fragmentation parameters and tunnel assignments based on network conditions and tunnel performance.

**Pseudocode (Client-Side Fragmenter):**

```
function process_packet(packet):
  flow_id = extract_flow_id(packet)
  fragment_id = get_next_fragment_id(flow_id)
  total_fragments = get_total_fragments(flow_id)
  
  tunnel_id = select_tunnel(flow_id) // Based on Round Robin, Hash, or App-Aware logic

  add_fragment_header(packet, fragment_id, total_fragments, tunnel_id)
  
  send_packet(packet, tunnel_id)

function select_tunnel(flow_id):
  if mode == "Round Robin":
    return next_tunnel_in_sequence()
  elif mode == "Hash":
    return hash(flow_id) % number_of_tunnels
  elif mode == "App-Aware":
    // Logic to select tunnel based on application type
    return best_tunnel_for_app(app_type)
```

**Potential Benefits:**

*   **Enhanced Throughput:**  Distributing traffic across multiple tunnels can increase overall throughput.
*   **Improved Resilience:**  If one tunnel fails, traffic can continue to flow over the remaining tunnels.
*   **Dynamic Bandwidth Allocation:**  Allows for more flexible bandwidth allocation based on real-time network conditions.
*   **Bypass Congestion:**  Traffic can be routed around congested areas of the network.