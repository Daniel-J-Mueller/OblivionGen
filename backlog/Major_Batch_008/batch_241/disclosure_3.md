# 10992975

## Predictive Fragment Prefetching with User Intent Modeling

**Specification:** A system for proactively fetching video fragments based not only on buffer levels and bitrate estimations, but also on predicted user viewing patterns and potential navigation actions.

**Core Concept:**  The existing patent focuses on *reacting* to buffer states with virtual buffers. This builds on that by *predicting* buffer needs based on user behavior and preemptively prefetching content.  It’s about shifting from buffering *against* stalls to preventing stalls before they’re even considered.

**Components:**

1.  **User Intent Module:** This module analyzes several data points to model user intent:
    *   **Viewing History:**  Tracks what the user has watched previously (content type, duration, time of day).
    *   **Navigation Patterns:** Logs how the user interacts with the streaming platform (e.g., fast-forwarding, rewinding, jumping to specific points, selecting different streams within a live event).  It also tracks dwell time on specific content segments.
    *   **Real-time Interaction:** Monitors current viewing behavior (current position, play/pause state, seeking behavior).  Includes monitoring of UI interactions *outside* the video player itself – indicating potential interruptions or shifts in attention.
    *   **External Data (Optional):** Incorporates external factors such as scheduled events (sports games) or social media trends to anticipate content demand.

2.  **Predictive Buffer Manager:** This module uses the User Intent Module’s output to forecast future buffer needs.
    *   **Behavioral Profiles:**  Creates profiles based on historical data, categorizing users into viewing 'types' (e.g., 'binge-watcher', 'live-event follower', 'casual browser').
    *   **Intent-Based Prediction:**  Based on the current user profile and real-time behavior, it predicts the likelihood of actions like fast-forwarding, rewinding, or switching streams.  For example, if a user frequently fast-forwards through commercials, the system will proactively fetch fragments that *follow* likely commercial breaks.  If a live event is nearing a critical moment (e.g., a penalty kick in soccer), it will aggressively prefetch fragments.
    *   **Dynamic Adjustment:**  The prediction model is continuously updated based on observed behavior.

3.  **Prefetching Engine:**  This component manages the prefetching of video fragments.
    *   **Priority-Based Queuing:** Fragments are queued for download based on their predicted need and priority. Higher priority is given to fragments likely needed immediately.
    *   **Bandwidth Management:** Prefetching is capped to avoid saturating the network connection.  The system dynamically adjusts the prefetch rate based on available bandwidth and network conditions.
    *   **Adaptive Fragmentation:** The system can request fragments of *variable* length.  If the model predicts a high probability of fast-forwarding through a segment, it may request a smaller fragment or even skip it entirely.

**Pseudocode (Prefetching Engine):**

```
function prefetch_fragments(user_intent_data, current_buffer_level, available_bandwidth) {
  predicted_needs = user_intent_data.get_predicted_fragment_requests();
  fragment_queue = new PriorityQueue();

  for (fragment_id in predicted_needs) {
    priority = predicted_needs[fragment_id].priority;
    fragment_size = predicted_needs[fragment_id].size;

    if (fragment_size <= available_bandwidth) {
      fragment_queue.enqueue(fragment_id, priority);
    }
  }

  while (!fragment_queue.isEmpty() && available_bandwidth > 0) {
    fragment_id = fragment_queue.dequeue();
    request_fragment(fragment_id);
    available_bandwidth -= get_fragment_size(fragment_id);
  }
}

function request_fragment(fragment_id) {
  // Logic to request fragment from server
}

function get_fragment_size(fragment_id) {
    // Logic to determine fragment size
}
```

**Novelty:**

This approach moves beyond reactive buffering to proactive prefetching, personalized to individual user behavior. The integration of intent modeling allows the system to anticipate viewing patterns and prefetch content *before* it’s needed, minimizing buffering interruptions and improving the overall viewing experience. The dynamic fragmentation capability further optimizes bandwidth usage.