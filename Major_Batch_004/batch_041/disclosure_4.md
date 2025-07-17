# 11343176

## Adaptive Request Splitting with Predictive QoS

**Concept:** Enhance interconnect efficiency by *proactively* splitting large requests into smaller, independently QoS-prioritized sub-requests *before* they reach the interconnect fabric. This differs from typical request queuing and prioritization by acting at the request *creation* stage, leveraging prediction of data access patterns.

**Specs:**

**1. Request Profiler Module:**

*   **Input:** Initial request metadata (address, size, type – read/write, completion callback).
*   **Function:** Analyze request metadata and historical data access patterns (maintained in a dedicated history buffer).  Employ a lightweight machine learning model (e.g., a shallow neural network or decision tree) trained on past requests. The model predicts the likelihood of *spatial locality* – the probability that adjacent memory locations will be accessed soon.
*   **Output:** A ‘split score’ (0.0 - 1.0) indicating the benefit of splitting the request.  Higher scores suggest greater benefit. Also outputs a suggested sub-request size, optimized for the interconnect’s bandwidth and latency characteristics.

**2. Adaptive Splitter Module:**

*   **Input:** Initial request, split score, suggested sub-request size.
*   **Function:** If the split score exceeds a configurable threshold, divide the request into multiple sub-requests.
    *   Sub-requests are generated with contiguous memory addresses.
    *   Each sub-request inherits the original request’s metadata *and* a dynamically assigned QoS value.
*   **Output:** A series of independent sub-requests. If the split score is below the threshold, the original request is passed through unmodified.

**3. Dynamic QoS Assignment:**

*   **Input:** Sub-request metadata, access pattern prediction data (from Request Profiler).
*   **Function:**  Assign QoS values based on predicted access patterns.
    *   If the Request Profiler predicts high probability of sequential access (spatial locality), assign *increasing* QoS values to subsequent sub-requests. This prioritizes the initial data fetch, reducing latency for the entire request.
    *   If access is predicted to be random, assign uniform QoS values or use a round-robin scheme.
*   **Output:** Modified sub-requests with assigned QoS values.

**4. Reassembly Logic:**

*   **Input:** Completed sub-requests, original request ID.
*   **Function:**  Collect completed sub-requests, verify data integrity, and reassemble the original data.
*   **Output:** Completed original request.

**Pseudocode (Adaptive Splitter Module):**

```
function split_request(request):
  split_score = Request_Profiler.get_split_score(request)
  if split_score > SPLIT_THRESHOLD:
    sub_request_size = Request_Profiler.get_suggested_sub_request_size(request)
    sub_requests = []
    start_address = request.address
    end_address = request.address + request.size
    while start_address < end_address:
      sub_request = create_sub_request(start_address, min(sub_request_size, end_address - start_address))
      sub_request.qos = Dynamic_QoS.assign_qos(sub_request)
      sub_requests.append(sub_request)
      start_address += sub_request_size
    return sub_requests
  else:
    return [request]
```

**Hardware Considerations:**

*   The Request Profiler module can be implemented using a dedicated hardware accelerator to minimize latency.
*   The QoS assignment logic can be integrated into the QoS regulator of the existing interconnect.
*   The Reassembly Logic may require additional cache memory to store partially completed sub-requests.