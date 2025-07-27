# 11481397

## Dynamic Audit Payload Enrichment

**Concept:** Extend the audit record payload with dynamically generated contextual data *before* batching and transmission. This moves beyond simply recording the query itself and allows for richer, more actionable audit trails.

**Specifications:**

**1. Contextual Data Sources:**

*   **System Performance Metrics:** Integrate real-time CPU usage, memory consumption, disk I/O, and network latency at the instance host *during* query execution.
*   **User Behavior Profiling:** Incorporate data from a pre-existing user behavior analysis system (if available) – identifying anomalous activity patterns for the user executing the query.  This could include deviation from typical query complexity, data access patterns, or time of day.
*   **Data Sensitivity Classification:**  Access a metadata service that provides data sensitivity classifications for the tables/columns accessed by the query.  (e.g., PII, Confidential, Public).
*   **Geographic Location:** Determine the originating IP address of the client and resolve it to a geographic location.

**2. Enrichment Pipeline:**

*   **Activity Monitor Hook:** The Activity Monitor plugin is extended to include a hook point *before* the audit record is written to shared memory.
*   **Enrichment Service:**  A dedicated microservice, the “Audit Enrichment Service” (AES), receives requests from the Activity Monitor.
*   **Asynchronous Enrichment:**  The Activity Monitor makes an *asynchronous* request to the AES, passing the base audit record data (query, timestamp, user ID, etc.).
*   **Data Gathering:** The AES gathers the relevant contextual data from the sources listed in Section 1.
*   **Payload Construction:** The AES constructs a new, enriched audit record payload. This can be a JSON object or a protocol buffer.
*   **Return to Activity Monitor:** The AES returns the enriched audit record to the Activity Monitor.
*   **Shared Memory Write:** The Activity Monitor writes the *enriched* audit record to shared memory.

**3. Data Structure (Enriched Audit Record – Example JSON):**

```json
{
  "timestamp": "2024-01-27T10:00:00Z",
  "user_id": "john.doe",
  "query": "SELECT * FROM customers WHERE city = 'New York'",
  "query_type": "SELECT",
  "database": "sales_db",
  "table_accessed": "customers",
  "data_sensitivity": "PII",
  "system_cpu_usage": 75.2,
  "system_memory_usage": 60.5,
  "system_disk_io": 12.8,
  "user_behavior_anomaly_score": 0.2,
  "originating_geo_location": {
    "country": "US",
    "city": "San Francisco"
  }
}
```

**4. Pseudocode (Activity Monitor Modification):**

```
function monitor_query(query_event):
  audit_record = create_base_audit_record(query_event)
  enriched_record = call_enrichment_service(audit_record) //Asynchronous call
  write_to_shared_memory(enriched_record)
```

**5. Considerations:**

*   **Performance Impact:**  Asynchronous calls minimize the impact on query performance.  Caching of contextual data (e.g., data sensitivity classifications) can further improve performance.
*   **Scalability:** The Audit Enrichment Service should be horizontally scalable to handle increased audit volume.
*   **Data Security:**  Ensure appropriate access controls and encryption for the enriched audit data.
*   **Configuration:** Allow administrators to configure which contextual data sources are enabled and the level of detail to be included.