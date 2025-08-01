# 10742586

## Encrypted Message ‘Time Capsules’

**Concept:** Extend the encrypted delivery concept to allow for messages to be ‘time-capsuled’ – scheduled for delivery only when *both* encryption standards are met *and* a specific date/time arrives. This leverages the existing infrastructure for encryption verification but adds a temporal constraint.

**Specs:**

*   **New Header Field:** Add a `Delivery-Time:` field to the email header. This field will contain a UTC timestamp representing the desired delivery date and time.
*   **Time Capsule Server:** Introduce a ‘Time Capsule Server’ – a dedicated component responsible for holding messages until both encryption criteria (as defined in the existing patent) *and* the `Delivery-Time` are satisfied.
*   **Modified Mail Server Interaction:** Mail servers along the delivery path, upon receiving a message with a `Delivery-Time` field, will:
    1.  Verify encryption support as per the existing patent.
    2.  If encryption is supported, forward the message to the Time Capsule Server.
    3.  If encryption is *not* supported, handle the message as in the existing patent (bounceback or alternative routing).
*   **Time Capsule Server Operation:**
    1.  Receives messages from mail servers.
    2.  Stores the message.
    3.  Continuously checks the current time against the `Delivery-Time` for each stored message.
    4.  When the current time matches or exceeds the `Delivery-Time` *and* encryption is confirmed to be viable across the path, forward the message to the next hop.
*   **Client-Side Implementation:** The email client will provide a UI element (e.g., a ‘Schedule Send’ option) that allows users to specify a future delivery date and time.  This will automatically add the `Delivery-Time:` header field.
*   **Policy Integration:** Extend existing policy definitions (mentioned in Claim 15) to include temporal restrictions.  This allows administrators to define periods where certain types of messages (or messages from specific senders) are allowed to be delivered.

**Pseudocode (Time Capsule Server):**

```
// Main loop
while (true) {
  foreach (Message msg in storedMessages) {
    if (currentTime >= msg.deliveryTime && encryptionValid(msg)) {
      forwardMessage(msg);
      removeMessage(msg);
    }
  }
  sleep(1 second);
}

function encryptionValid(Message msg) {
  //Check if encryption standards are met across path
  //Use existing patent verification logic
  return isValid;
}

function forwardMessage(Message msg) {
  //Forward message to next hop
}

function removeMessage(Message msg) {
  //Remove message from storage
}
```

**Potential Applications:**

*   Delayed confidential disclosures (e.g., contract releases).
*   Scheduled delivery of marketing campaigns based on optimal timing.
*   Automated notifications with specific time-based triggers.
*   ‘Dead man’s switch’ functionality – messages delivered only if no action is taken by the sender within a specified timeframe.