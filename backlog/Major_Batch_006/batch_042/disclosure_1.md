# 11108626

## Dynamic Network Personality Injection

**Concept:** Extend the network rewriting functionality to allow for "network personality" injection – dynamically altering network behavior *as if* the virtual machine is experiencing different network conditions or is operating within a different network topology.

**Specification:**

**1. Personality Profiles:**

*   Define a data structure – `NetworkPersonality` – containing configurable network parameters:
    *   `latencyMs`: Integer, simulated network latency in milliseconds.
    *   `packetLossPercentage`: Float (0.0-1.0), simulated packet loss percentage.
    *   `bandwidthKbps`: Integer, simulated network bandwidth limitation.
    *   `duplicatePacketPercentage`: Float (0.0-1.0), simulates occasional duplicated packets.
    *   `jitterMs`: Integer, network jitter (variation in latency).
    *   `routingProfile`: Enum {Static, Dynamic, Chaotic} - Defines how traffic is routed *within* the virtual network. `Static` uses predefined routes, `Dynamic` adjusts based on simulated load, and `Chaotic` introduces random route changes.
    *   `mtu`: Integer, Maximum Transmission Unit for simulated network.

**2. Personality Manager Component:**

*   New component: `PersonalityManager`. Runs alongside `CommunicationManager` and `SystemManager`.
*   Receives instructions from a central control plane (API/UI) specifying a `NetworkPersonality` and the target VM instance.
*   Stores mapping of VM instance ID to active `NetworkPersonality`.

**3. Communication Manager Modification:**

*   Before rewriting the header, `CommunicationManager` queries `PersonalityManager` for the active `NetworkPersonality` of the destination VM.
*   If a `NetworkPersonality` is defined, apply the following modifications *before* header rewriting:
    *   **Latency Simulation:** Introduce a delay equal to `latencyMs` using a time-based scheduling mechanism.
    *   **Packet Loss:** Randomly discard packets with probability `packetLossPercentage`.
    *   **Bandwidth Limiting:** Implement a token bucket algorithm to limit outgoing traffic to `bandwidthKbps`.
    *   **Duplicate Packet Injection:** Randomly duplicate packets with probability `duplicatePacketPercentage`.
    *   **Jitter Simulation:** Add random variations to the latency delay, up to `jitterMs`.
    *   **Routing Profile Application:** If `routingProfile` is `Dynamic` or `Chaotic`, dynamically adjust the destination IP address within the virtual network before forwarding the packet.  `Chaotic` routing introduces a small probability of sending the packet to a random, valid IP address within the subnet.

**4. API Extension:**

*   Add API endpoints to:
    *   `POST /v1/personalities`:  Create a new `NetworkPersonality` profile.
    *   `PUT /v1/personalities/{personalityId}`: Update an existing `NetworkPersonality` profile.
    *   `POST /v1/vms/{vmId}/personalities/{personalityId}`: Assign a `NetworkPersonality` to a VM.
    *   `DELETE /v1/vms/{vmId}/personalities`: Remove a `NetworkPersonality` assignment from a VM.

**Pseudocode (CommunicationManager Modification):**

```
function processCommunication(communication, header):
  personality = PersonalityManager.getPersonality(header.destinationVMId)

  if personality != null:
    header = applyNetworkPersonality(header, personality)

  rewrittenHeader = rewriteHeader(header) // Existing functionality
  provideRewrittenCommunication(rewrittenHeader, communication)

function applyNetworkPersonality(header, personality):
  // Simulate latency, packet loss, bandwidth limiting, jitter
  // Implement routing adjustments based on personality.routingProfile
  return adjustedHeader
```

**Potential Use Cases:**

*   **Realistic Network Testing:**  Simulate various network conditions for application testing.
*   **Failure Injection:** Introduce controlled network failures to test application resilience.
*   **Geographic Simulation:**  Emulate network performance from different geographic locations.
*   **Security Auditing:**  Simulate attacks based on network conditions.
*   **Developer Environments:** Create isolated network environments with custom conditions.