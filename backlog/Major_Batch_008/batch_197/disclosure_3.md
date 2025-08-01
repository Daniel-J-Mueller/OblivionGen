# 9116909

## Adaptive Fingerprint Granularity & Predictive Caching

**System Specification:** A module integrated into both the data sender (customer gateway) and data receiver (virtualized data store). This module dynamically adjusts the granularity of fingerprint creation and leverages predictive caching based on observed access patterns.

**Core Concept:** The patent focuses on deduplication via fingerprinting. This adaptation moves beyond fixed-size fingerprinting and introduces *dynamic* fingerprint granularity, coupled with a predictive cache on the sender-side. It’s predicated on the idea that data access isn’t uniform. Small, frequently changing files benefit from finer granularity, while larger, static files benefit from coarser granularity. 

**Module 1: Sender-Side Predictive Cache & Granularity Engine**

*   **Data Monitoring:** Continuously monitors access patterns for locally cached data. Tracks file size, modification frequency, and access frequency.
*   **Granularity Profile Creation:** Based on monitoring data, assigns each data unit (or group of data units) a granularity profile.
    *   **Profile Levels:**  Fine (byte-level), Medium (block-level, e.g., 4KB), Coarse (file-level).
    *   **Adaptive Thresholds:** Thresholds for switching between profile levels are dynamically adjusted based on observed access patterns.  (e.g. If a 4KB block is modified >5 times per hour, switch to fine granularity).
*   **Fingerprint Generation:** Generates fingerprints based on the assigned granularity profile for each data unit.
*   **Predictive Cache:** Leverages access patterns to predict which data units will be accessed next. Prefetches these data units (or portions thereof, based on granularity) and caches them locally.
*   **Bandwidth Allocation Request:** Estimates bandwidth needed for prefetching and uploads, and proactively requests bandwidth allocation.
*   **Communication with Receiver:** Transmits granularity profile alongside fingerprints.  This informs the receiver how fingerprints were generated.

**Module 2: Receiver-Side Fingerprint Index & Granularity Awareness**

*   **Granularity Index:**  The fingerprint dictionary is extended to include the granularity level associated with each fingerprint.
*   **Granularity Matching:** When receiving fingerprints, the receiver uses the associated granularity level to perform accurate matching. It won’t attempt to match a byte-level fingerprint with a block-level fingerprint.
*   **Fingerprint Decomposition/Aggregation:**  If a fingerprint match isn’t found at the exact granularity, the receiver attempts decomposition (splitting a coarser fingerprint into finer ones) or aggregation (combining finer fingerprints into a coarser one) to find a match.  This adds complexity, but improves deduplication efficiency.
*   **Cache Poisoning Mitigation:**  Receives updates from the sender regarding data modifications. Proactively invalidates or updates cached fingerprints and data based on these updates.

**Pseudocode (Sender - Core Granularity Engine):**

```
function determineGranularity(dataUnit, accessFrequency, modificationFrequency):
    if accessFrequency > HIGH_THRESHOLD and modificationFrequency < LOW_THRESHOLD:
        return GRANULARITY_COARSE
    elif accessFrequency < LOW_THRESHOLD and modificationFrequency > HIGH_THRESHOLD:
        return GRANULARITY_FINE
    else:
        return GRANULARITY_MEDIUM

function generateFingerprint(dataUnit, granularity):
    if granularity == GRANULARITY_FINE:
        return hash(dataUnit) // Byte-level hash
    elif granularity == GRANULARITY_MEDIUM:
        return hash(dataUnit.getBlock()) // Block-level hash
    else:
        return hash(dataUnit.getFile()) // File-level hash
```

**Hardware Implications:**  Increased processing requirements on both sender and receiver for granularity analysis and fingerprint manipulation. Potential need for hardware acceleration (e.g., dedicated hashing engines).

**Benefits:**

*   Improved deduplication rates by adapting to data characteristics.
*   Reduced bandwidth usage through proactive caching and optimized fingerprinting.
*   Enhanced responsiveness for frequently accessed data.
*   Greater scalability for large-scale virtualized data storage.