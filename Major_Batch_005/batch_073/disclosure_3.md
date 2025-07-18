# 10037428

**Ephemeral Key Exchange via Distributed Ledger Technology (DLT)**

**Concept:** Extend the request-supplied key concept by leveraging a DLT (blockchain) for ultra-short-lived key exchange, drastically minimizing the window of vulnerability. Instead of transmitting an encrypted key *with* the request, the request contains instructions for a smart contract to *generate* a unique, symmetric key.

**Specs:**

*   **Smart Contract (SC) Deployment:** A series of pre-deployed SCs exist on a permissioned DLT (e.g., Hyperledger Fabric, Corda). These SCs are specialized for key derivation.
*   **Request Format:** The request includes:
    *   `Request ID`: A unique identifier for the request.
    *   `SC Address`: The address of the designated key derivation SC.
    *   `Salt`: A randomly generated salt included in the request.
    *   `Data Pointer`:  An identifier pointing to the data to be operated on (stored externally – object storage, database, etc.).
    *   `TTL`: Time-To-Live (in seconds) – defines the lifespan of the key.
*   **Key Derivation Process:**
    1.  The request is received.
    2.  The system extracts the `SC Address`, `Salt`, and `TTL`.
    3.  The system calls the specified SC. The SC receives the `Salt` and `TTL`.
    4.  The SC derives a symmetric key using a cryptographically secure key derivation function (KDF) seeded with the `Salt` and a pre-shared secret known *only* to the SC and the data storage system.
    5.  The SC *does not* return the key. Instead, it records the derived key's hash (for auditing) and the `TTL` on the DLT.
    6.  The system receives confirmation from the SC (a transaction ID on the DLT).
    7.  The system instructs the data storage system to use the derived key (identified by its hash) to perform the requested operation on the data pointed to by the `Data Pointer`. The storage system obtains the key based on the hash.
    8.  After the `TTL` expires, the storage system automatically purges the key. The SC also purges any related state.
*   **Data Storage System Integration:**
    *   The storage system must support key identification by hash.
    *   The storage system must enforce the `TTL` and automatically purge keys.
*   **Security Considerations:**
    *   The pre-shared secret within the SC must be highly secure. Hardware Security Modules (HSMs) are recommended.
    *   The DLT must be permissioned to control access to the SC.
    *   Auditing: All key derivation and access events are recorded on the DLT for forensic analysis.

**Pseudocode (Request Processing):**

```
function processRequest(request) {
  scAddress = request.scAddress;
  salt = request.salt;
  dataPointer = request.dataPointer;
  ttl = request.ttl;

  transactionId = callSmartContract(scAddress, salt, ttl); // Invoke SC

  storageSystem.operateOnData(dataPointer, transactionId); // Use hash as key identifier

  // SC automatically handles key expiration
}
```

**Innovation:**  The system eliminates the need to transmit an encrypted key, significantly reducing the attack surface. The combination of short-lived keys, DLT immutability, and automated key management provides a highly secure and auditable data protection mechanism. It's a departure from symmetric key exchange, focusing on ephemeral key *generation* at the point of use.