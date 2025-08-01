# 11003684

**Adaptive Data Shadowing with Predictive Relocation**

**Concept:** Extend the core idea of managing changes to database pages to proactively create ‘shadows’ of frequently accessed data closer to the requesting compute node *before* a request is even made. This minimizes latency by anticipating data needs, and drastically reduces contention.

**Specifications:**

1.  **Data Usage Monitoring:** Implement a system-level monitoring agent on each compute node. This agent tracks data access patterns (page IDs, access frequency, data modification times) and reports this information to a central ‘Predictive Relocation Manager’ (PRM).

2.  **Predictive Relocation Manager (PRM):**  The PRM uses machine learning models (e.g., LSTM, time series forecasting) to predict future data access.  It identifies pages with high predicted access probability. 

3.  **Shadow Page Creation:** When the PRM predicts high access probability for a page, it initiates the creation of a ‘shadow page’ – a copy of the page – on a storage device physically closer to the requesting compute node (e.g., local SSD, in-memory cache on a nearby server).  Shadow pages are versioned, tying them to specific modification timestamps.

4.  **Request Interception & Redirection:** Incoming data requests are intercepted by a ‘Request Manager’ on the compute node. 

    *   The Request Manager checks if a valid shadow page exists for the requested data, matching the required version (modification timestamp).
    *   If a valid shadow page exists, the request is immediately served from the shadow page.
    *   If no shadow page exists (or the version is outdated), the request is handled as normal (fetching from the primary storage).

5.  **Write Propagation & Invalidation:** When a page is modified on the primary storage:

    *   A ‘write notification’ is sent to all compute nodes that have shadow copies of that page.
    *   Compute nodes invalidate their shadow copies or initiate a refresh of the shadow page from the primary storage (using a background synchronization process).

6.  **Contention Mitigation:** 

    *   If the PRM predicts high contention for a page, it can create *multiple* shadow copies on different storage devices, acting as a basic form of replication.
    *   The Request Manager can dynamically switch between these shadow copies to balance the load.

**Pseudocode (Request Manager):**

```
function handle_data_request(page_id, timestamp):
  shadow_page = get_shadow_page(page_id, timestamp)
  if shadow_page is valid:
    return shadow_page.data
  else:
    data = fetch_from_primary_storage(page_id)
    create_shadow_page(page_id, data) # Create shadow page for future requests
    return data
```

**Pseudocode (PRM – Simplified Prediction):**

```
function predict_access(page_id, history):
  # Calculate access frequency over the last 'n' minutes
  frequency = count_accesses(page_id, history) 
  # If frequency exceeds threshold, predict high access
  if frequency > threshold:
    return True
  else:
    return False
```

**Storage Considerations:**

*   Shadow pages can be stored on a mix of storage tiers (SSD, NVMe, memory) based on predicted access frequency and latency requirements.
*   A ‘garbage collection’ mechanism is needed to remove outdated or unused shadow pages.

**Benefits:**

*   Significantly reduced latency for frequently accessed data.
*   Reduced contention on the primary storage.
*   Improved application responsiveness.
*   Scalability.