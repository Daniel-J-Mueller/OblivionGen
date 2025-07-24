# 12271276

## Dynamic Endpoint Masking & Regional Health Scoring

**Concept:** Enhance failover responsiveness and granularity beyond simple region switching by dynamically masking endpoint identifiers *during* failover and implementing a regional health scoring system that influences routing decisions *before* a full failover event.

**Specs:**

**1. Endpoint Masking Module (EMM):**

*   **Function:** Intercepts network packets originating from/destined for a specific region. Modifies the endpoint identifier (IP address, hostname, etc.) based on pre-defined masking rules and current regional health scores.
*   **Implementation:**  Software module integrated into the first/second computing devices (as described in the patent).  Utilizes a lookup table mapping original endpoint IDs to masked IDs.
*   **Masking Types:**
    *   *Full Masking:* Replaces the endpoint ID with a static, pre-configured ID in the failover region. Useful for stateless services.
    *   *Partial Masking:* Modifies only specific parts of the endpoint ID (e.g., subnet). Useful for maintaining some locality during failover.
    *   *Dynamic Masking:*  Generates a unique masked ID based on a cryptographic hash of the original ID and a timestamp.  Improves security and reduces replay attacks.
*   **Configuration:** Masking rules are defined via a central management plane and pushed to all EMM instances.

**2. Regional Health Scoring (RHS) System:**

*   **Metrics:** RHS collects various metrics to assess the health of each region:
    *   *Latency:* Ping times between regions and to critical services.
    *   *Packet Loss:* Percentage of packets lost during communication.
    *   *Resource Utilization:* CPU, memory, and disk usage on computing devices in the region.
    *   *Application Health:*  Health checks performed on running applications. (HTTP status codes, database connection status)
*   **Scoring Algorithm:**  A weighted sum of the collected metrics. Weights are configurable via the central management plane.
*   **Thresholds:**  Pre-defined thresholds for health scores. Scores below a certain threshold trigger alerts and/or initiate failover procedures.
*   **Proactive Routing:**  RHS can influence routing decisions *before* a full failover event. If a region’s health score is degrading, the system can dynamically reroute a portion of the traffic to a healthier region.

**3.  Integration with Existing System:**

*   The EMM and RHS modules are integrated into the first and second computing devices described in the patent.
*   The first computing device (primary region) uses RHS to monitor regional health and initiates failover based on pre-defined thresholds.
*   During failover, the first computing device pushes the current RHS data (health scores, masking rules) to the second computing device (failover region).
*   The second computing device uses the received data to route traffic and apply endpoint masking.

**Pseudocode (First Computing Device - Failover Initiation):**

```
function checkRegionalHealth() {
  healthScore = calculateHealthScore();
  if (healthScore < threshold) {
    initiateFailover();
  }
}

function initiateFailover() {
  pushRHSDataToFailoverRegion();
  routeAllTrafficToFailoverRegion();
}
```

**Pseudocode (Second Computing Device - Traffic Routing during Failover):**

```
function receiveTraffic(packet) {
  if (packet.endpointIdentifier == originalEndpointIdentifier) {
    maskedEndpointIdentifier = applyEndpointMasking(originalEndpointIdentifier);
    routePacketToDestination(maskedEndpointIdentifier);
  } else {
    routePacketToDestination(packet.endpointIdentifier);
  }
}
```

**Novelty:** This system moves beyond simple region-based failover to provide more granular control over traffic routing during failover. Dynamic endpoint masking can improve security and reduce the impact of failover on applications. Regional health scoring allows for proactive routing and can prevent full-scale failovers in many cases. It enables a sliding scale of regional engagement, rather than a hard flip.