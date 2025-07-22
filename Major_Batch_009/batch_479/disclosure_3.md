# 10243819

## Automated Resource ‘Drift’ Detection & Remediation

**Concept:** Extend the template-based resource provisioning to *actively* monitor provisioned resources for deviations from the defined template (drift), and automatically remediate or alert based on configurable policies. This goes beyond initial provisioning – it’s ongoing governance.

**Specs:**

**1. Drift Detection Agent:**

*   **Deployment:** Sidecar container alongside each provisioned resource (VM, storage, network).
*   **Function:** Periodically (configurable interval - 5m, 15m, 30m, 1hr) captures the *current* state of the resource configuration (e.g., firewall rules, mounted volumes, IAM roles, software versions).
*   **State Capture Methods:** OS-level configuration files, API calls to resource providers (AWS, Azure, GCP), snapshotting configuration data.
*   **Hashing:** Generates a cryptographic hash (SHA256) of the captured state.
*   **Reporting:** Sends the hash, resource ID, and timestamp to a central Drift Management Service.

**2. Drift Management Service:**

*   **Template Storage:** Stores the original template definitions (JSON/YAML).
*   **Hash Generation (Baseline):**  Generates a baseline hash from the original template.
*   **Comparison:**  Compares the received resource hash with the baseline template hash.
*   **Deviation Threshold:**  Configurable sensitivity for drift detection. Small changes (e.g., log rotation settings) might be ignored; significant changes (e.g., firewall rule changes) trigger alerts.
*   **Alerting:** Sends alerts (email, Slack, PagerDuty) when drift exceeds the threshold. Includes details about the affected resource and the nature of the detected drift.
*   **Remediation Policies:**  Defines automated remediation actions:
    *   **Auto-Correct:**  If possible, automatically revert the resource configuration to match the template. (Requires idempotent operations).
    *   **Quarantine:** Isolate the resource (e.g., remove from load balancer) until reviewed.
    *   **Manual Review:**  Create a ticket for a human operator to investigate.

**3. API Integration:**

*   **Template API:** Allows programmatic creation, update, and deletion of templates.
*   **Drift API:**  Allows querying drift status for specific resources.
*   **Remediation API:**  Allows triggering remediation actions programmatically.

**Pseudocode (Remediation Policy Engine):**

```
function evaluate_policy(resource_id, detected_drift, policy_definition) {
  if (policy_definition.type == "auto_correct") {
    try {
      execute_correction_script(resource_id, policy_definition.script);
      log("Auto-corrected resource " + resource_id);
      return true;
    } catch (error) {
      log("Auto-correction failed for " + resource_id + ": " + error);
      return false;
    }
  } else if (policy_definition.type == "quarantine") {
    quarantine_resource(resource_id);
    log("Quarantined resource " + resource_id);
    return true;
  } else if (policy_definition.type == "manual_review") {
    create_ticket(resource_id, detected_drift);
    log("Created ticket for manual review of resource " + resource_id);
    return true;
  } else {
    log("Unknown policy type");
    return false;
  }
}
```

**Data Model (Simplified):**

*   **Template:** `template_id`, `resource_type`, `template_definition` (JSON/YAML), `created_at`, `updated_at`.
*   **DriftReport:** `report_id`, `resource_id`, `template_id`, `timestamp`, `drift_hash`, `drift_details` (JSON – diff between current and template state).
*   **RemediationPolicy:** `policy_id`, `template_id`, `drift_threshold`, `action_type` (auto_correct, quarantine, manual_review), `script` (for auto-correct).

This system expands the proactive benefits of the template system by adding continuous monitoring and automated response, effectively creating ‘self-healing’ infrastructure.