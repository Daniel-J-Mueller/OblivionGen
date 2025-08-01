# 9641598

## Contextual Identifier 'Lifespan' Profiles

**Specification:** A system to allow for granular control over identifier lifespan based on pre-defined, dynamically adjustable ‘profiles’ linked to customer accounts, beyond simple time-based expiration.

**Concept:** The patent establishes timed deletion. This system expands on that by allowing profiles to dictate deletion based on multiple factors *in addition* to time. This facilitates more nuanced security and data management.

**Components:**

1.  **Profile Definition Module:**  Allows administrators (or, potentially, customer account owners with permissions) to create identifier lifespan profiles.  These profiles define rules for deletion based on a weighted combination of factors.

2.  **Factor Weighting Engine:**  Calculates a ‘lifespan score’ for each identifier based on the current values of weighted factors.  Factors include, but are not limited to:

    *   **Time Since Generation:** (Weight: 0-1.0) The traditional time-based expiration.
    *   **Usage Count:** (Weight: 0-1.0)  How many times the identifier has been used for authentication/authorization. Lower usage = higher deletion priority.
    *   **Data Sensitivity Level:** (Weight: 0-1.0) A pre-defined sensitivity level associated with the data the identifier protects. Higher sensitivity = faster deletion.
    *   **Geographic Location:** (Weight: 0-1.0) A list of approved/disapproved geographic locations where the identifier is expected to be used. Usage outside of approved locations increases deletion priority.
    *   **Device Fingerprint:** (Weight: 0-1.0)  A fingerprint of the device used to initially generate/use the identifier. Use from a different device increases deletion priority.
    *   **Risk Score (External):** (Weight: 0-1.0) Integration with an external risk scoring service (e.g., fraud detection) to adjust lifespan based on real-time risk assessment.

3.  **Deletion Policy Engine:**  Compares the calculated lifespan score to a pre-defined threshold. If the score exceeds the threshold, the identifier and associated data are marked for deletion.

4.  **Dynamic Adjustment Module:**  Allows administrators to dynamically adjust factor weights and thresholds in real-time.  This enables rapid response to changing security threats or business requirements.

**Pseudocode (Deletion Policy Engine):**

```
function calculate_lifespan_score(identifier, customer_account) {
    time_since_generation_score = get_time_since_generation(identifier) * weight_time
    usage_count_score = (1.0 / (get_usage_count(identifier) + 1.0)) * weight_usage
    sensitivity_score = customer_account.data_sensitivity * weight_sensitivity
    geo_location_score = get_geo_location_score(identifier) * weight_geo
    device_score = get_device_score(identifier) * weight_device
    risk_score = get_external_risk_score(identifier) * weight_risk

    lifespan_score = time_since_generation_score + usage_count_score + sensitivity_score + geo_location_score + device_score + risk_score
    return lifespan_score
}

function determine_deletion(identifier, customer_account, deletion_threshold) {
    score = calculate_lifespan_score(identifier, customer_account)
    if (score > deletion_threshold) {
        mark_for_deletion(identifier)
        return true
    }
    return false
}
```

**Integration with Existing System:**

The existing patent establishes the core identifier generation and deletion timeline. This system adds a layer of complexity *before* that timed deletion happens.  The ‘Deletion Policy Engine’ would be integrated into the existing system, checking the lifespan score *before* marking an identifier for deletion based solely on time. This adds more granular control and improves security posture.