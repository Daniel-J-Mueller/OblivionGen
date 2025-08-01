# 9660970

## Decentralized Key Attestation with Bloom Filters

**Concept:** Extend the HSM fleet key management to incorporate a decentralized attestation system leveraging Bloom filters. This allows for proving membership in a "trusted fleet" *without* revealing specific key material or requiring centralized verification. It addresses potential scaling issues with centralized key distribution and enhances resilience against single points of failure.

**Specifications:**

**1. Fleet Membership & Bloom Filter Generation:**

*   Each HSM, upon joining the fleet, generates a unique identifier (HSM-ID).
*   A “Fleet Authority” (could be a designated HSM or external system) maintains a Bloom filter representing the currently authorized HSM-IDs. The filter’s size is determined by the expected fleet size and acceptable false positive rate.
*   The Fleet Authority periodically broadcasts (or makes available) the current Bloom filter to all HSMs in the fleet.  This broadcast utilizes a secure channel (e.g., TLS with mutual authentication).
*   HSMs *do not* store a complete list of authorized HSMs, only the Bloom filter.

**2. Key Request & Attestation:**

*   When an HSM (Requesting HSM) needs to establish a secure connection with another HSM (Target HSM), the Requesting HSM initiates a key exchange request.
*   As part of the request, the Requesting HSM generates a *challenge* (random nonce).
*   The Target HSM signs the challenge with its private key.
*   *Before* transmitting the signed challenge, the Target HSM calculates a hash of its HSM-ID. It *then* checks if this hash is present in the locally stored Bloom filter. 
*   If the HSM-ID hash is *not* in the Bloom filter, the Target HSM rejects the connection request *without* revealing any key material.
*   If the HSM-ID hash *is* present, the Target HSM transmits the signed challenge.
*   The Requesting HSM verifies the signature. Successful signature verification, combined with the positive Bloom filter check, confirms the Target HSM’s authenticity *and* fleet membership.

**3. Dynamic Bloom Filter Updates:**

*   When an HSM joins or leaves the fleet, the Fleet Authority updates the Bloom filter.
*   The Fleet Authority broadcasts the *difference* (a delta) between the old and new Bloom filters. HSMs apply this delta to their local Bloom filters, minimizing bandwidth usage.
*   Bloom filter updates are cryptographically signed by the Fleet Authority to prevent tampering.
*   HSMs maintain a timestamp of the last Bloom filter update. If an HSM hasn’t received an update within a defined period, it should temporarily suspend accepting new connections.

**4. Revocation:**

*   HSMs can be *soft-revoked* by adding their HSM-ID to a revocation list maintained by the Fleet Authority. 
*   HSMs download the revocation list periodically.
*   Before accepting a connection, an HSM checks if the connecting HSM’s ID is present in the revocation list.

**Pseudocode (HSM - Accepting Connection):**

```
function acceptConnection(request):
  if isRevoked(request.HSM_ID):
    rejectConnection()
    return

  if not verifySignature(request.signedChallenge, request.HSM_ID):
    rejectConnection()
    return

  HSM_ID_Hash = hash(request.HSM_ID)
  if not bloomFilterContains(HSM_ID_Hash):
    rejectConnection()
    return

  acceptConnection()
```

**Security Considerations:**

*   Bloom filter false positives are possible.  The filter size must be chosen carefully to minimize this risk.
*   The Fleet Authority is a critical component.  Its security must be robust.
*   The Bloom filter update mechanism must be secure against man-in-the-middle attacks.
*   Revocation list distribution must be reliable and timely.