# 10116593

## Dynamic Topology-Aware BGP Advertisement via Encrypted Broadcast

**Concept:** Extend the core idea of propagating BGP information within an IGP to include dynamic, localized topology awareness *and* introduce a layer of encrypted broadcast for enhanced security and resilience. Instead of simply translating BGP data into TLVs for IGP distribution, this system actively shapes the BGP advertisements based on *local* network conditions and broadcasts them with rolling encryption keys.

**Specifications:**

**1. Node Types:**

*   **Edge Nodes:** Responsible for receiving BGP information, processing it, and generating encrypted broadcast messages.
*   **Interior Nodes:** Receive and decrypt broadcast messages, updating their local routing tables, and potentially re-broadcasting with modified parameters.
*   **Sink Nodes:** Designated nodes that collect aggregated routing information for external consumption.

**2. Data Structures:**

*   **Encrypted BGP Packet:**
    *   Header: Version, Packet Type, TTL, Sequence Number
    *   Payload: Encrypted BGP data (NLRI, Path Attributes)
    *   Encryption Key ID: Identifier for current encryption key
*   **Topology Map:** Each node maintains a localized map of its direct and near neighbors. This map includes:
    *   Neighbor IDs
    *   Link Quality (bandwidth, latency, loss)
    *   Current Encryption Key ID assigned to neighbor

**3. Algorithm – Edge Node Operation:**

```pseudocode
function process_bgp_message(bgp_message):
  # Extract BGP data (NLRI, Path Attributes)
  extracted_data = extract_bgp_data(bgp_message)

  # Create Encrypted BGP Packet
  packet = create_encrypted_bgp_packet(extracted_data)

  # Determine Neighbors for Broadcast (based on Topology Map)
  neighbors = get_neighbors(Topology Map)

  # For each neighbor:
  for neighbor in neighbors:
    # Select/Generate Encryption Key (rolling key scheme)
    key = select_encryption_key(neighbor)

    # Encrypt Payload using Key
    encrypted_payload = encrypt(extracted_data, key)

    # Set Key ID in Packet
    packet.KeyID = key.ID

    # Transmit Packet to Neighbor
```

**4. Algorithm – Interior Node Operation:**

```pseudocode
function receive_packet(packet):
  # Verify Packet Integrity
  if packet.integrity_check() == False:
    return

  # Decrypt Payload using Key ID
  decrypted_data = decrypt(packet.Payload, packet.KeyID)

  # Update Local Routing Table
  update_routing_table(decrypted_data)

  # Determine Neighbors for Re-Broadcast (based on Topology Map & Link Quality)
  neighbors = get_neighbors(Topology Map)

  # For each neighbor:
  for neighbor in neighbors:
    # If Link Quality exceeds threshold:
    if link_quality(neighbor) > threshold:
      # Re-broadcast packet
      transmit(packet, neighbor)
```

**5. Key Management:**

*   **Rolling Key Scheme:** Each Edge Node generates a unique encryption key for each neighbor, and these keys are periodically rotated. This limits the impact of key compromise.
*   **Key Distribution:**  Keys can be distributed using a Diffie-Hellman key exchange or a pre-shared key mechanism.
*   **Key ID:** Each key is assigned a unique ID to facilitate decryption.

**6. Topology Awareness:**

*   **Link Quality Monitoring:** Nodes continuously monitor link quality to neighbors.
*   **Adaptive Broadcasting:** Nodes prioritize broadcasting to neighbors with higher link quality.
*   **Path Selection:** The routing algorithm incorporates link quality and encryption key freshness into path selection.



**7. Sink Node Operation:**

Sink nodes aggregate the received BGP information from Interior Nodes and construct a consolidated view of the network topology. This information can then be used for network monitoring, optimization, and policy enforcement.