# 12200118

## Dynamic Key Pair ‘Lifecycles’ & Reputation System

**Concept:** Extend the pre-generated key pair system to incorporate a lifecycle and reputation score, influencing future key pair selection and potentially flagging compromised keys.

**Specifications:**

**1. Key Pair Lifecycle Management:**

*   **States:** Define key pair states: *New*, *Active*, *Suspended*, *Revoked*.
*   **Transitions:**
    *   *New* -> *Active*: Upon initial allocation to a request.
    *   *Active* -> *Suspended*: Triggered by anomaly detection (see section 3).  Key pair remains usable but flagged for monitoring.
    *   *Active/Suspended* -> *Revoked*:  Triggered by explicit revocation request (admin or client) or exceeding a defined anomaly threshold.
*   **Data Storage:** Each key pair record includes:  *Creation Timestamp*, *Last Used Timestamp*, *Current State*, *Anomaly Score* (initially 0), *Usage Count*.

**2. Reputation System & Weighted Selection:**

*   **Scoring:**  Assign a reputation score to each queue of pre-generated key pairs based on historical usage and associated anomaly scores of keys drawn from that queue.
*   **Weighting Algorithm:**
    *   `Queue Reputation = (Total Usage Count - Total Anomaly Score) / Total Usage Count`
    *   If `Total Usage Count` is zero, assign a default reputation score (e.g., 0.5).
*   **Selection Probability:** During key pair allocation, implement a weighted random selection algorithm.  Queues with higher reputation scores have a higher probability of being selected.
*   **Dynamic Adjustment:** Reputation scores are recalculated periodically (e.g., hourly) or in response to significant events (e.g., detection of compromised key).

**3. Anomaly Detection & Feedback Loop:**

*   **Monitoring Metrics:** Track key pair usage for anomalies, including:
    *   *Request Frequency Spikes*: Unexpectedly high number of requests using a specific key pair within a short timeframe.
    *   *Geographic Location Variance*: Requests originating from unusual or diverse geographic locations.
    *   *Data Payload Patterns*: Unusual patterns in data encrypted/decrypted using the key pair.
*   **Anomaly Scoring:**  Assign an anomaly score to each key pair based on detected anomalies. Higher scores indicate a greater likelihood of compromise.
*   **State Transition:**  If the anomaly score exceeds a defined threshold, transition the key pair to the *Suspended* or *Revoked* state.
*   **Feedback to Reputation:**  Incorporate the anomaly score into the queue reputation calculation, penalizing queues that generate anomalous keys.

**Pseudocode (Key Pair Allocation):**

```
function allocateKeyPair(request):
  queueReputations = calculateQueueReputations()
  totalWeight = sum(queueReputations.values())

  randomValue = generateRandomNumber(0, totalWeight)
  cumulativeWeight = 0

  selectedQueue = null
  for queue, weight in queueReputations:
    cumulativeWeight += weight
    if cumulativeWeight >= randomValue:
      selectedQueue = queue
      break

  keyPair = removeKeyPairFromQueue(selectedQueue)
  keyPair.lastUsedTimestamp = currentTime()
  keyPair.state = "Active"
  return keyPair
```

**Data Structures:**

*   **KeyPair:** { publicKey, privateKey, creationTimestamp, lastUsedTimestamp, state, anomalyScore, usageCount }
*   **Queue:** { queueId, algorithmType, keyPairs }
*   **QueueReputation:** { queueId, reputationScore }