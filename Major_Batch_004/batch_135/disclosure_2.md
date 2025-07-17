# 10979535

## Decentralized Impression Verification Network

**Core Concept:** A blockchain-based system for verifying impression events on semi-connected devices *before* retroactive cost calculations. This moves verification from solely server-side to a distributed, tamper-proof network.

**Specifications:**

*   **Device Component:** Semi-connected devices (phones, tablets, IoT) run a lightweight “Impression Witness” module. This module doesn't store full content data, only cryptographic hashes of displayed content and precise timestamp data.
*   **Blockchain:** A permissioned blockchain (potentially a sidechain of a larger network) designed for high throughput and low cost. Nodes operate at the edge (on semi-connected devices themselves, or nearby access points).
*   **Impression Witness Operation:**
    1.  When content is displayed, the Witness module calculates a hash of the displayed content (image, video, text snippet - only the visually presented portion).
    2.  The Witness module captures a precise timestamp (using NTP or similar).
    3.  A “Proof of Impression” transaction is created:
        *   Transaction Data: Content Hash, Timestamp, Device ID (pseudonymized), Campaign ID.
        *   Signature: Digitally signed by the device's private key.
    4.  The transaction is broadcast to the blockchain network.
*   **Server Component:**
    1.  Receives initial content delivery confirmation (as in the original patent).
    2.  Monitors blockchain for “Proof of Impression” transactions related to its campaigns.
    3.  Validates transactions:
        *   Signature verification.
        *   Confirms Device ID is a registered device.
        *   Cross-references content hash with the content originally served.
    4.  Uses validated blockchain data *instead* of solely relying on reconnection data to calculate retroactive costs.
*   **Cost Calculation:**
    *   If a “Proof of Impression” transaction is found on the blockchain: cost is calculated using the data in the transaction (timestamp, content ID).
    *   If no “Proof of Impression” is found: impression is considered invalid, and no cost is applied.

**Pseudocode (Server Component - Cost Calculation):**

```
function calculate_impression_cost(device_id, content_id, delivery_timestamp) {
  blockchain_data = query_blockchain(device_id, content_id, delivery_timestamp);

  if (blockchain_data.found) {
    impression_timestamp = blockchain_data.timestamp;
    bid_amount = get_bid_amount(content_id, impression_timestamp); //Lookup bid amount at impression time
    return bid_amount;
  } else {
    return 0; // Impression not verified
  }
}
```

**Edge Node Specifications:**

*   Lightweight blockchain client.
*   Transaction validation and relay capabilities.
*   Secure key storage.
*   Optimized for low power consumption.

**Novelty:** This shifts impression verification from a centralized server process to a decentralized, tamper-proof network, reducing fraud and increasing transparency.  It uses blockchain not for data *storage* of content, but for reliable *verification* of impressions.