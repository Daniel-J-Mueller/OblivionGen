# 10013273

## Dynamic Instance 'Health' Based Termination Prevention

**Concept:** Extend the safety stock/threshold idea beyond simple instance *count* to incorporate a dynamic ‘health’ score for each instance, preventing termination of instances critical for ongoing workload performance.

**Specification:**

**1. Health Score Calculation:**

*   Each virtual machine instance is assigned a ‘Health Score’ ranging from 0-100.
*   Health Score is calculated by a dedicated monitoring service.
*   Factors contributing to Health Score:
    *   **CPU Utilization:** (25% weighting) - Higher, consistent utilization = higher score.
    *   **Memory Utilization:** (25% weighting) - Similar to CPU, but with allowances for caching.
    *   **I/O Operations/Second:** (20% weighting) - High, *sustained* I/O = higher score.  Spikes are ignored.
    *   **Application-Specific Metrics:** (30% weighting) -  This is configurable per application. Examples:  Messages processed per second (queue worker), transactions completed per second (database), active connections (web server). The provisioning service receives these metrics from the application itself via API.
*   Metrics are averaged over a rolling window (e.g., 5 minutes) to smooth out fluctuations.
*   Weights are configurable at a global level, and potentially per application tier.

**2. Termination Request Interception & Analysis:**

*   When a termination request is received, the provisioning service queries the health score for each instance in the request.
*   The system defines ‘Health Tiers’ (e.g., Critical, Healthy, Degraded).
*   Each tier has an associated ‘Termination Risk Multiplier’. 
    *   Critical: 1.0 (Termination strongly discouraged)
    *   Healthy: 0.5 (Normal risk)
    *   Degraded: 0.25 (Lower risk, but still consider)
*   The system calculates a ‘Weighted Termination Risk Score’ for the entire request:

    ```
    TotalRiskScore = SUM(HealthTierMultiplier * InstanceCount)
    ```

**3. Dynamic Threshold Adjustment:**

*   Instead of a fixed safety stock threshold, the system dynamically adjusts the threshold based on the Weighted Termination Risk Score.
*   A base threshold is established (e.g., minimum 5 instances).
*   The dynamic adjustment is calculated as:

    ```
    AdjustedThreshold = BaseThreshold + (WeightedTerminationRiskScore * AdjustmentFactor)
    ```

    (AdjustmentFactor is a configurable parameter.)

**4. Termination Workflow:**

*   If the number of instances to be terminated *plus* the AdjustedThreshold exceeds the current running instance count, the termination request is automatically blocked. An alert is generated for the administrator.
*   If the AdjustedThreshold is *not* exceeded, the system proceeds with the normal termination confirmation workflow (if any).
*   The system logs the AdjustedThreshold, WeightedTerminationRiskScore, and termination decision for auditing and analysis.

**5. API Endpoints:**

*   `/health/instance/{instance_id}`: Returns the Health Score for a specific instance.
*   `/health/request/{request_id}`: Returns the AdjustedThreshold and WeightedTerminationRiskScore for a termination request.
*   `/config/health/weights`:  Allows configuration of Health Score weights and AdjustmentFactor.

**6.  Scalability Considerations:**

*   Health Score calculation should be asynchronous and non-blocking to avoid impacting termination request processing.
*   Health Score data should be cached to minimize database queries.
*   Monitoring service should be horizontally scalable to handle a large number of instances.