# 10650003

## Adaptive Credential Entropy

**Concept:** Expand the probabilistic data structure beyond simple expiration tracking to dynamically assess credential *entropy* – a measure of how predictable or compromised a credential might be, influencing access control beyond just time-based expiration.

**Specification:**

**1. Data Structure Augmentation:**

*   Extend the probabilistic data structure (e.g., Bloom filter) to store not just a "last used" timestamp (credential iteration value), but also a *credential health score*. This score will be a floating-point value ranging from 0.0 (highly compromised) to 1.0 (highly secure).
*   The Bloom filter entries will need to be modified to accommodate this health score, perhaps by utilizing multiple bit arrays or a more complex data structure within each Bloom filter "cell."

**2. Entropy Calculation:**

*   **Input Features:** The health score will be calculated based on a variety of features:
    *   **Time Since Last Use:** (As in the original patent) – Longer inactivity lowers the score.
    *   **Credential Complexity:** (New) – A measure of password strength (length, character variety).  This requires initial assessment upon credential creation/registration.
    *   **Breach History:** (New) – Integrate with known breach databases. If the credential (or a similar one) appears in a breach, dramatically lower the score.
    *   **Geolocation Risk:** (New) – Track login locations.  Logins from unusual or high-risk locations lower the score.
    *    **Device Fingerprinting:** (New) – Establish a 'trusted device' baseline. New or unusual device fingerprints reduce the score.

*   **Calculation:** Use a weighted scoring system.  Assign weights to each feature based on its perceived importance. Combine the weighted feature values to produce the overall health score. The weights can be dynamically adjusted through machine learning based on observed usage patterns and security events.

**3. Adaptive Access Control:**

*   **Thresholds:** Define access control thresholds based on the health score:
    *   **High Health (e.g., 0.8-1.0):** Full access granted.
    *   **Medium Health (e.g., 0.5-0.8):**  Multi-factor authentication (MFA) required.
    *   **Low Health (e.g., 0.0-0.5):** Access denied.  Credential reset required.

*   **Dynamic Adjustment:**  The access control thresholds can be dynamically adjusted based on the overall system security posture and the criticality of the resource being accessed.

**4. System Architecture**

*   **Entropy Calculation Service:**  A dedicated service responsible for calculating the credential health score based on the input features. This service would need access to breach databases, geolocation data, and device fingerprinting information.
*   **Bloom Filter Integration:** The Bloom filter will store the calculated health score alongside the last used timestamp.
*   **Access Control Policy Engine:**  The policy engine will retrieve the health score and timestamp from the Bloom filter and enforce the access control policy.

**Pseudocode (Entropy Calculation Service):**

```
function calculate_credential_health(credential, last_used_timestamp, login_location, device_fingerprint):
  complexity_score = calculate_complexity(credential)  // Score 0-1
  breach_score = check_breach_databases(credential) //Score 0-1 (0 if breached, 1 if not)
  location_score = assess_location_risk(login_location) // Score 0-1
  device_score = assess_device_trust(device_fingerprint) // Score 0-1

  //Weighted average (weights can be adjusted via ML)
  health_score = (0.3 * complexity_score) + (0.4 * breach_score) + (0.15 * location_score) + (0.15 * device_score)

  //Clamp between 0 and 1
  health_score = max(0, min(1, health_score))

  return health_score
```

**Novelty:**  This expands the system from simply tracking time-based expiration to a more holistic assessment of credential risk, enabling more granular and adaptive access control.  It’s a move toward proactive security rather than reactive expiration.