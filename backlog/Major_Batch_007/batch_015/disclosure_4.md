# 12095909

## Temporal Key Derivation & ‘Shadow’ Validation

**Concept:** Extend the core idea of validating re-encrypted keys by introducing time-based key derivation and a ‘shadow’ validation system. This addresses potential long-term key compromise and enhances auditability.

**Specification:**

**1. Temporal Key Derivation:**

*   **Primary Key:** The ‘customer master key’ as described in the reference material.
*   **Epochs:** Time is divided into discrete epochs (e.g., 1 hour, 1 day, adjustable).
*   **Derivation Function:** A Key Derivation Function (KDF) – Argon2id recommended – takes the primary key *and* the current epoch as input, producing a ‘current key’.
*   **Epoch Clock:** A highly accurate, tamper-resistant clock (hardware-based recommended) determines the current epoch. Synchronization with a trusted time source is critical.
*   **Key Rotation Policy:** Define a maximum epoch lifespan before automatic key rotation.
*   **Encryption:** All data encrypted using the *current key*, not the primary key.

**2. Shadow Validation System:**

*   **Shadow Keys:**  Alongside each ‘current key’, a ‘shadow key’ is derived. The shadow key is generated using a *different* KDF and a unique, randomly generated salt (associated with the epoch).
*   **Shadow Encryption:** A small portion of the encrypted data (metadata, audit logs) is *also* encrypted using the shadow key. This ‘shadow data’ is separate from the primary encrypted data.
*   **Validation Procedure:**
    *   When validating a re-encrypted key, *after* the standard random value verification, attempt to decrypt the shadow data using the derived shadow key.
    *   Successful decryption of the shadow data confirms:
        *   The re-encryption process is functioning correctly.
        *   The key derivation process is consistent.
        *   The clock is not significantly skewed.
    *   Failure of shadow decryption triggers a high-severity alert and initiates emergency key rotation/revocation.

**3.  Implementation Details:**

*   **Hardware Security Modules (HSMs):** All key derivation, encryption, and decryption operations *must* be performed within HSMs.
*   **Audit Logging:**  Detailed audit logs track all key derivation events, shadow encryption/decryption attempts, and validation outcomes.
*   **API Endpoints:**
    *   `deriveKey(primaryKey, epoch)`:  Returns the derived key for a given epoch. (HSM-protected).
    *   `validateReencryption(reencryptedKey, epoch)`:  Performs re-encryption validation including shadow decryption. Returns a boolean success/failure.
    *   `getKeyEpoch(key)`: Returns the epoch a key was derived for.

**Pseudocode (validation flow):**

```
function validateReencryption(reencryptedKey, epoch):
    // 1. Standard Random Value Verification (as in the reference material)
    if not verifyRandomValue(reencryptedKey):
        return false

    // 2. Derive Shadow Key
    shadowKey = deriveShadowKey(primaryKey, epoch)

    // 3. Attempt Shadow Decryption
    try:
        decryptedShadowData = decryptShadowData(shadowKey, shadowEncryptedData)
    except DecryptionError:
        // Log decryption failure
        return false

    // 4. Validation Success
    return true
```

**Potential Benefits:**

*   **Enhanced Security:** Mitigates long-term key compromise by limiting the exposure of any single key to a finite epoch.
*   **Improved Auditability:** Shadow decryption provides a strong indicator of system integrity.
*   **Clock Skew Detection:**  Failed shadow decryption can signal clock drift.
*   **Automated System Health Checks:**  Regular shadow decryption attempts can serve as automated health checks for the entire key management infrastructure.