# 10592428

## Adaptive TLB Granularity via Dynamic Hashing

**Specification:** Implement a system to dynamically adjust the granularity of TLB invalidation based on observed access patterns. The current patent focuses on pointer-based invalidation down to individual TLB entries. This expands upon that concept with a probabilistic, multi-level hashing system *within* the IPA translation buffer to provide coarser or finer granularity of invalidation.

**Components:**

*   **IPA Translation Buffer (ITB):** Modified to incorporate a multi-level hash table structure.
*   **Hash Level Select Register (HLSR):** A register within the processor controlling the depth of hashing used in the ITB.
*   **Access Pattern Monitor (APM):** Hardware dedicated to tracking access patterns to IPAs.
*   **Granularity Adjustment Unit (GAU):** Logic that uses data from the APM to dynamically adjust the HLSR.

**Operation:**

1.  **Initial State:** The HLSR is initialized to a default value, establishing a default hashing depth (granularity). For example, a depth of 0 could mean the entire TLB is invalidated with a single pointer, while a higher depth allows for invalidation of individual entries.

2.  **Access Pattern Monitoring:** The APM continuously monitors IPA accesses. It tracks:
    *   **Collision Rate:** How often different IPAs map to the same hash bucket at a given HLSR value.
    *   **Temporal Locality:** How frequently the same IPA is accessed within a short time window.
    *   **Spatial Locality:**  How often consecutive IPAs are accessed.

3.  **Granularity Adjustment:** The GAU analyzes the APM data.
    *   **High Collision Rate:** If the collision rate is high at the current HLSR, it indicates that coarse-grained invalidation is causing unnecessary flushes of valid TLB entries. The GAU *increases* the HLSR to refine the granularity.
    *   **High Temporal/Spatial Locality:** If temporal/spatial locality is high, it suggests that coarser granularity is acceptable without significant performance loss. The GAU *decreases* the HLSR.
    *   **Dynamic Thresholds:** The GAU utilizes configurable thresholds for the metrics to prevent excessive adjustments.

4.  **ITB Structure:**  The ITB now stores not just pointers to TLB entries, but also metadata related to the hash bucket the IPA belongs to. This enables the translation manager to invalidate an entire hash bucket (a set of TLB entries) rather than individual entries.

**Pseudocode (GAU Logic):**

```
// Configuration parameters
const int MAX_HASH_LEVEL = 5;
const int COLLISION_THRESHOLD = 80; // Percentage
const int LOCALITY_THRESHOLD = 90; // Percentage

// Metrics from APM
int collisionRate;
int temporalLocality;
int spatialLocality;

int currentHashLevel; // From HLSR

function adjustHashLevel() {
  if (collisionRate > COLLISION_THRESHOLD && currentHashLevel < MAX_HASH_LEVEL) {
    currentHashLevel++; // Increase granularity
    HLSR = currentHashLevel;
  } else if (temporalLocality > LOCALITY_THRESHOLD && spatialLocality > LOCALITY_THRESHOLD && currentHashLevel > 0) {
    currentHashLevel--; // Decrease granularity
    HLSR = currentHashLevel;
  }
}

// Main Loop (executed periodically)
while(true){
    collisionRate = APM.getCollisionRate();
    temporalLocality = APM.getTemporalLocality();
    spatialLocality = APM.getSpatialLocality();

    adjustHashLevel();

    wait(100ms);
}
```

**Benefits:**

*   **Adaptive Performance:** Automatically optimizes TLB invalidation granularity based on runtime access patterns.
*   **Reduced TLB Pollution:** Minimizes unnecessary TLB flushes, improving performance.
*   **Improved Power Efficiency:** Fewer TLB flushes translate to lower power consumption.
*   **Scalability:** Adaptable to various workloads and system configurations.