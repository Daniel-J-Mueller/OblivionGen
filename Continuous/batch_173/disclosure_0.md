# 9116909

## Adaptive Fingerprint Granularity

**System Specifications:**

*   **Core Component:** Granularity Controller Module (GCM) - Software module integrated within both the data receiver and data sender.
*   **Data Structures:**
    *   *Granularity Profile:* A configurable data structure defining the minimum and maximum fingerprint granularity (data unit size) for a given customer or data type.  Includes a 'dynamic adjustment' flag.
    *   *Granularity History:*  A rolling window storing recent fingerprint hit/miss rates for each customer and granularity level.
*   **Hardware Requirements:** Minimal. Existing processor and memory resources of the data receiver and data sender are sufficient.  Potential benefit from hardware acceleration for hashing functions.

**Functional Description:**

The core innovation is *dynamic adjustment of fingerprint granularity*.  Instead of a fixed data unit size for fingerprinting, the system adapts the granularity based on observed data access patterns.

1.  **Initial Configuration:** Each customer (or data type) begins with a pre-defined Granularity Profile (e.g., minimum 4KB, maximum 64KB). The profile can be based on anticipated usage patterns.
2.  **Monitoring:** The GCM monitors fingerprint hit/miss rates for each customer at various granularity levels (within the defined profile).  This is done passively as part of the existing fingerprint comparison process.
3.  **Dynamic Adjustment:**
    *   If hit rates are consistently high at a larger granularity, the system *increases* the minimum granularity.  This reduces the number of fingerprints to store and transmit.
    *   If hit rates are consistently low at a larger granularity, the system *decreases* the minimum granularity. This improves the likelihood of finding existing data, potentially reducing uploads.
    *   The adjustment happens gradually, and is constrained by the Granularity Profile.
4.  **Feedback Loop:** The system maintains a Granularity History to prevent oscillations and to adapt to changing data access patterns over time. The dynamic adjustment flag, when enabled, allows the controller to switch between static and dynamic modes.

**Pseudocode (GCM – Data Receiver Side):**

```
function process_fingerprint_request(customer_id, fingerprint_list) {
    granularity_profile = get_granularity_profile(customer_id)
    current_granularity = granularity_profile.minimum

    for each fingerprint in fingerprint_list {
        hit = search_fingerprint_dictionary(fingerprint, current_granularity)
        if hit {
            // Data exists - record hit rate
            update_hit_rate(customer_id, current_granularity, 1)
        } else {
            // Data does not exist - record miss rate
            update_hit_rate(customer_id, current_granularity, 0)
        }
    }

    // Evaluate and adjust granularity (e.g., every N requests)
    if (requests_since_last_adjustment > N) {
        hit_rate = get_average_hit_rate(customer_id, current_granularity)
        if (hit_rate > threshold_high) {
            increase_granularity(customer_id, granularity_profile)
        } else if (hit_rate < threshold_low) {
            decrease_granularity(customer_id, granularity_profile)
        }
        reset_adjustment_timer()
    }
}
```

**Potential Benefits:**

*   **Reduced Bandwidth:** Optimizing fingerprint granularity reduces the overall data transmitted.
*   **Improved Performance:** Smaller granularity increases the chance of cache hits.
*   **Adaptive to Workloads:** Adapts to changing data access patterns without manual configuration.
*   **Scalability:**  Optimized fingerprint storage reduces the load on the data receiver.