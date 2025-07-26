# 11341179

## Dynamic Watermarking with Perceptual Hash Drift

**Concept:** Extend the artifact insertion concept (Claim 17, 27) beyond static watermarks to a dynamic system where 'artifacts' are perceptual hashes that intentionally *drift* over time, coupled with a blockchain-based registry. This creates a verifiable history of media authenticity *and* subtle, detectable tampering.

**Specs:**

**1. Perceptual Hash Generation & Drift Profiles:**

*   **Perceptual Hash Algorithm:** Employ a robust perceptual hash algorithm (pHash) that captures high-level visual/audio features.  This should be configurable (different algorithms available) to balance robustness & sensitivity.
*   **Drift Profile Definition:**  Introduce the concept of a "Drift Profile" associated with each registered source media.  This profile defines:
    *   *Drift Rate:*  How much the pHash is allowed to change over a defined period (e.g., 1% per day).
    *   *Drift Vector:* The *direction* of the drift. This could be random, pseudo-random based on a seed, or deterministic based on a key.  This ensures a predictable but not easily replicated evolution of the hash.
    *   *Hash Resolution:*  Number of bits in the hash. Higher resolution offers finer granularity, but increases computational cost.
*   **Initial Hash Embedding:** Embed the initial pHash directly into the source media via a steganographic technique.  This serves as the baseline for drift tracking.
*   **Temporal Hash Calculation:** At predefined intervals (e.g., daily, weekly), recalculate the pHash based on the Drift Profile.  The new hash is *not* directly embedded. Instead, it's stored off-chain (see Blockchain Integration).

**2. Blockchain Integration:**

*   **Off-Chain Hash Storage:**  Store the sequence of calculated pHashes (from Temporal Hash Calculation) in a decentralized storage network (e.g., IPFS, Filecoin). The storage location (CID) is recorded on the blockchain.
*   **Blockchain Registry:**  Maintain a blockchain-based registry containing:
    *   Source Media ID (unique identifier)
    *   Drift Profile (Drift Rate, Drift Vector, Hash Resolution)
    *   Initial Hash (embedded in source media)
    *   CID of the off-chain hash storage.
*   **Tamper Detection:**
    1.  Obtain the suspected altered media.
    2.  Extract the initial hash.
    3.  Query the blockchain for the Source Media ID and Drift Profile.
    4.  Retrieve the sequence of expected hashes from the off-chain storage.
    5.  Calculate the expected hash at the suspected alteration time, based on the Drift Profile.
    6.  Compare the calculated expected hash to the extracted initial hash.  Significant divergence indicates tampering.
    7.  Recalculate expected hashes going forward and compare against extracted hashes.  Consistent divergence across time confirms tampering.

**3. System Architecture:**

*   **Registration Service:**  Responsible for registering source media, defining Drift Profiles, and embedding the initial hash.
*   **Verification Service:**  Handles tamper detection, querying the blockchain, retrieving hashes, and performing comparisons.
*   **Blockchain Node:**  Manages the blockchain registry.
*   **Decentralized Storage Integration:**  Connects to a decentralized storage network.

**Pseudocode (Verification Service - Tamper Detection):**

```
function verifyMedia(mediaFile):
  initialHash = extractInitialHash(mediaFile)
  sourceMediaID = identifySourceMedia(mediaFile) // Could use metadata, visual fingerprinting
  driftProfile = getDriftProfile(sourceMediaID) // From Blockchain
  hashSequenceCID = getHashSequenceCID(sourceMediaID) // From Blockchain
  hashSequence = retrieveHashSequence(hashSequenceCID) // From Decentralized Storage

  expectedHash = calculateExpectedHash(initialHash, driftProfile, alterationTime)

  if (distance(expectedHash, initialHash) > threshold):
    return "Tampered"

  // Check subsequent hashes against calculated expected hashes
  for (each hash in hashSequence):
    calculatedHash = calculateExpectedHash(initialHash, driftProfile, hash.timestamp)
    if (distance(calculatedHash, hash) > threshold):
      return "Tampered"

  return "Authentic"

```

**Novelty:**

This system goes beyond static watermarking to a dynamic, time-evolving authenticity verification system. The intentional hash drift provides a verifiable history, making tampering more difficult to conceal. The blockchain ensures the integrity of the Drift Profile and the hash sequence. The system can function even if the initial embedded hash is slightly altered, as long as the drift pattern remains consistent. This is useful in cases where minor, authorized edits are made to the original media.