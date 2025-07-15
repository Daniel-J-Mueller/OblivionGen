# 10025628

## Adaptive Message Decay & Prioritization

**Concept:** Extend the replicated queue system to incorporate message decay and dynamic prioritization based on observed processing latency and client-defined importance. This moves beyond simple replication for high availability to intelligent data lifecycle management *within* the queue.

**Specification:**

**1. Message Metadata Enhancement:**

*   Each message will include the following additions:
    *   `Importance Level`: Integer (1-10, 10 = highest) – Client-defined.
    *   `Initial TTL (Time-To-Live)`: Duration – Initial lifespan of the message.
    *   `Decay Rate`: Float (0.0-1.0) – Determines how quickly the `Importance Level` diminishes over time.  Higher rates indicate faster decay.
    *   `Processing Latency History`: Array of Timestamps – Records the time taken to process the message across different queue nodes.
    *   `Acknowledgement Count`: Integer – Tracks how many queue nodes have acknowledged the message.

**2. Queue Node Behavior Modification:**

*   **Enqueue:** Upon receiving a message, the queue node performs the standard replication.  It also initializes the `Processing Latency History` and `Acknowledgement Count`.
*   **Decay Process (Background Task):**  Each queue node will run a background task that periodically (e.g., every 5 seconds) iterates through its queued messages and performs the following:
    *   Calculates a `Current Importance` based on: `Current Importance = Importance Level * (1 - Decay Rate * (Current Time – Enqueue Time))`
    *   Updates the `Processing Latency History` with processing times observed on that node.
*   **Prioritized Dequeue:** When a client requests a message, the queue nodes will select the message with the highest `Current Importance` among those with the most complete `Acknowledgement Count`.  If multiple messages have the same `Current Importance`, the node will select the one with the shortest `Processing Latency History` (favoring messages that haven’t been repeatedly retried).

**3. Acknowledgement & Data Removal Protocol:**

*   Queue nodes *only* remove a message when:
    *   The message has been successfully processed by the client.
    *   The client sends an acknowledgement containing the message ID.
    *   *All* replicated nodes have received the acknowledgement.
*   If a message's `Current Importance` falls below a threshold (e.g., 1) *before* processing, the message is automatically marked as “Stale” and removed from the queue. This prevents the queue from being filled with obsolete data.  A log entry is created for each removed stale message.

**4. Adaptive Decay Rate Adjustment:**

*   A centralized "Queue Manager" service monitors the overall processing latency across all queue nodes.
*   If the average latency exceeds a predefined threshold, the Queue Manager sends a signal to *increase* the `Decay Rate` for *newly enqueued* messages. This accelerates the decay of less critical messages, prioritizing those with higher importance.
*   If latency decreases, the Queue Manager reduces the `Decay Rate`.

**Pseudocode (Queue Node - Decay Process):**

```
function DecayProcess() {
  for each message in queue {
    currentTime = getCurrentTime()
    age = currentTime - message.enqueueTime
    message.currentImportance = message.importanceLevel * (1 - message.decayRate * age)

    if (message.currentImportance < STALE_THRESHOLD) {
      logStaleMessage(message.id)
      removeMessage(message.id)
    }
  }
}

function dequeueMessage() {
  highestImportance = -1
  selectedMessage = null

  for each message in queue {
    if (message.currentImportance > highestImportance) {
      highestImportance = message.currentImportance
      selectedMessage = message
    } else if (message.currentImportance == highestImportance && message.processingLatencyHistory.length < selectedMessage.processingLatencyHistory.length) {
      selectedMessage = message
    }
  }

  return selectedMessage
}
```

**Benefits:**

*   **Intelligent Prioritization:** Ensures that critical messages are processed first, even under high load.
*   **Reduced Storage Costs:** Automatic removal of stale messages prevents the queue from filling up with obsolete data.
*   **Dynamic Adaptation:** The system automatically adjusts to changing load conditions and message importance.
*   **Improved System Responsiveness:** Prioritizing critical messages leads to faster processing times for those messages.