# 10313225

## Dynamic Route Attestation via Blockchain

**Concept:** Extend the routing information exchange to incorporate a permissioned blockchain for attestation of route validity and origin, enhancing security and trust within the provider network and between providers/customers.

**Specifications:**

**1. Blockchain Integration:**

*   **Type:** Permissioned blockchain (e.g., Hyperledger Fabric, Corda) to control participation and maintain data privacy.
*   **Nodes:** Routing service nodes act as blockchain validator nodes.  Each routing device capable of participating runs a lightweight client.
*   **Data Structure:**  “Route Transaction” – Contains route information (destination, next hop, metrics), originating routing device ID, timestamp, digital signature, and a hash of the RIB entry it corresponds to.

**2. Route Advertisement Process:**

*   **Standard Advertisement:** Routing devices still advertise routes via existing protocols (BGP, OSPF, etc.).
*   **Blockchain Commitment:** Simultaneously, the routing device creates a "Route Transaction" and commits it to the blockchain. The transaction includes a cryptographic hash of the RIB entry it's advertising.
*   **RIB Validation:** The routing service, upon receiving a route advertisement, *first* validates the corresponding “Route Transaction” on the blockchain.
    *   Verification of originating device's signature.
    *   Confirmation that the hash of the advertised route matches the hash stored in the blockchain transaction.
    *   Time-based validity check (prevent replay attacks).

**3.  RIB Synchronization & Conflict Resolution:**

*   The routing service maintains a local blockchain copy to quickly verify route authenticity.
*   In cases of conflicting routes (different next hops for the same destination), the blockchain's transaction history is used as the source of truth. The most recent, validated transaction wins.
*   A dispute resolution mechanism allows network operators to flag suspicious routes for manual review.

**4.  Dynamic Trust Scoring:**

*   Each routing device's “trust score” is calculated based on:
    *   Frequency of successful route validations.
    *   History of route corrections/withdrawals.
    *   Participation in dispute resolution.
*   The routing service prioritizes routes from devices with higher trust scores. This could be integrated into path selection algorithms.

**5. API Extensions:**

*   New API endpoints for:
    *   Querying route validation status on the blockchain.
    *   Submitting dispute requests.
    *   Retrieving trust scores.

**Pseudocode (Routing Service Node):**

```
function receive_route_advertisement(advertisement):
  transaction_hash = advertisement.transaction_hash
  transaction = blockchain.get_transaction(transaction_hash)

  if transaction is valid and matches advertisement:
    route_info = advertisement.route_info
    update_RIB(route_info)
    update_FIB(route_info)
    update_device_trust(advertisement.source_device, success)
  else:
    log_invalid_route(advertisement.source_device)
    update_device_trust(advertisement.source_device, failure)

function submit_route_transaction(route_info):
  transaction = create_transaction(route_info, my_device_id, timestamp)
  sign_transaction(transaction, my_private_key)
  blockchain.submit_transaction(transaction)
```

**Potential Benefits:**

*   Enhanced security against route hijacking and man-in-the-middle attacks.
*   Improved trust and reliability of routing information.
*   Greater transparency and accountability within the provider network.
*   Facilitates secure inter-provider routing agreements.