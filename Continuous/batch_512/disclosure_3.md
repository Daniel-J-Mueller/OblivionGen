# 10623186

## Dynamic Contextual Key Derivation & Sharding

**Concept:** Extend the multi-context encryption by *deriving* envelope keys dynamically from a combination of master key, resource identifiers, and *time-based* context. Then, *shard* the body data, encrypting each shard with a *different* derived envelope key. 

**Rationale:**  The original patent focuses on static context. This design introduces temporal context and sharding, significantly increasing security and resilience against compromise. If one envelope key is compromised, only a *portion* of the data is exposed, and the compromise is time-limited.  Dynamic key derivation makes pre-computation attacks vastly more difficult.

**Specs:**

**1. Key Derivation Function (KDF):**

   *   **Input:** Master Key (MK), Resource Identifier (RI), Timestamp (TS – epoch seconds with millisecond precision), Data Shard Index (DSI).
   *   **Process:**
        1.  Concatenate MK, RI, TS, and DSI into a single string.
        2.  Apply a robust KDF (e.g., HKDF with SHA-512) to this string.
        3.  Output: Derived Envelope Key (DEK) – a symmetric encryption key.

**2. Data Sharding & Encryption:**

   *   **Input:** Body Data (BD), Master Key (MK), Resource Identifiers (RIs – list), Number of Shards (NS).
   *   **Process:**
        1.  Divide BD into NS equal-sized shards (BD1, BD2… BDn).  If BD size is not divisible by NS, pad the last shard.
        2.  For each shard BD<i>:
            1.  Obtain current timestamp TS.
            2.  Determine appropriate Resource Identifier RI from the RIs list (based on shard index <i>, or a pre-defined mapping).
            3.  Generate DEK using KDF(MK, RI, TS, <i>).
            4.  Encrypt shard BD<i> using DEK (symmetric encryption – AES-256-GCM recommended).
            5.  Store encrypted shard (ES<i>).
        6.  Concatenate all encrypted shards (ES1, ES2…ESn) to form Encrypted Body Data (EBD).

**3. Metadata Structure:**

   *   **Header:** Includes:
        *   Master Key Identifier (MKI) –  to locate the associated master key.
        *   Resource Identifier List (RIL) – list of available resource identifiers.
        *   Number of Shards (NS).
        *   Shard Size (SS).
        *   Padding Indicator (PI) –  boolean indicating if padding was applied.
   *   Data: EBD

**4. Decryption Process:**

   *   **Input:** Encrypted Data, Master Key, Resource Identifier List
   *   **Process:**
        1.  Extract header information (MKI, RIL, NS, SS, PI).
        2.  For each shard:
            1.  Determine the correct resource identifier RI from RIL based on shard index.
            2.  Obtain current timestamp.  *Crucially*, decrypt uses the *original* timestamp when the data was encrypted, not the current time. This timestamp *must* be stored alongside the encrypted data.
            3.  Generate DEK using KDF(MK, RI, Original Timestamp, Shard Index).
            4.  Decrypt the shard using DEK.
        5.  Concatenate decrypted shards to reconstruct the body data.
        6.  Remove padding (if PI is true).

**Pseudocode (Decryption):**

```
function decryptData(encryptedData, masterKey, resourceList, originalTimestamp) {
  header = extractHeader(encryptedData);
  numShards = header.numShards;
  shardSize = header.shardSize;

  decryptedBody = "";
  for (i = 0; i < numShards; i++) {
    shard = extractShard(encryptedData, i * shardSize, shardSize);
    dek = kdf(masterKey, resourceList[i], originalTimestamp, i);
    decryptedShard = decrypt(shard, dek);
    decryptedBody += decryptedShard;
  }

  return decryptedBody;
}
```

**Innovation Highlights:**

*   **Temporal Context:** The inclusion of timestamps significantly increases the attack surface required for decryption, as the correct time is necessary.
*   **Key Sharding:** Distributes the risk by utilizing multiple derived keys, limiting the impact of a single key compromise.
*   **Dynamic Derivation:** Eliminates reliance on pre-shared keys, reducing the potential for static key attacks.
*   **Resilience:**  Even if a subset of resource identifiers is compromised, the remaining shards remain secure (assuming correct timestamps).