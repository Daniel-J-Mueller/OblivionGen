# 9613120

## Temporal Data Sharding with View-Specific Materialization

**Concept:** Expand the idea of "views" beyond simple data subsets to encompass *temporal* slices of the database, coupled with proactive materialization for read-only nodes. This addresses performance bottlenecks when querying historical data and supports time-travel debugging/auditing.

**Specification:**

**1. Temporal Sharding Layer:**

*   Introduce a temporal sharding layer *above* the existing common data store. This layer doesn’t physically move data, but rather adds metadata indicating the "valid time" range for each data record.
*   Each record in the common data store has associated `start_ts` and `end_ts` timestamps. `end_ts` can be `infinity` for currently active data.
*   The temporal sharding layer maintains an index mapping time ranges to the corresponding data within the common data store.

**2. View Definitions & Materialization Profiles:**

*   "Views" are redefined as a combination of data subset (standard SQL view) *and* a time range.
*   Each view has an associated "materialization profile":
    *   `materialization_strategy`:  `ON_DEMAND`, `PROACTIVE`, `HYBRID`.
    *   `refresh_interval`: (If `PROACTIVE` or `HYBRID`)  How often the materialization is refreshed.
    *   `replication_factor`:  How many read-only nodes should have this view materialized.
    *   `staleness_tolerance`: Maximum acceptable delay between the source data and the materialized view.

**3. Read-Only Node View Management:**

*   Upon startup (or on-demand), a read-only node requests view definitions from the master node.
*   Based on the materialization profile and available resources, the master node assigns views to the read-only node for materialization.
*   The read-only node queries the common data store based on the view definition *and* the `start_ts` and `end_ts` values.
*   Materialized data is stored locally at the read-only node, optimized for query performance.

**4. Change Propagation & View Updates:**

*   The master node tracks all data modifications.
*   For each modification, it determines which materialized views are affected.
*   It pushes invalidation messages to the corresponding read-only nodes.
*   Read-only nodes can either:
    *   Re-query the common data store to rebuild the affected portion of the view.
    *   Apply incremental updates based on change logs (if available).

**Pseudocode (Read-Only Node Startup):**

```
function startup():
  view_definitions = request_view_definitions_from_master()
  assigned_views = master.assign_views(view_definitions, node.resources)

  for view in assigned_views:
    if view.materialization_strategy == "PROACTIVE":
      materialized_data = query_common_data_store(view.sql, view.start_ts, view.end_ts)
      store_materialized_data(view.name, materialized_data)
    elif view.materialization_strategy == "ON_DEMAND":
      // Materialization happens only when a query is received
      pass

  // Start listener for invalidation messages from the master node
```

**Innovation Highlights:**

*   **Temporal Isolation:** Allows querying the database as it existed at a specific point in time, crucial for auditing, debugging, and compliance.
*   **Proactive Materialization:** Reduces query latency by pre-computing frequently accessed views, particularly for historical data.
*   **Scalability:** Distributes the workload of view maintenance across multiple read-only nodes.
*   **Flexibility:** Supports a range of materialization strategies to optimize performance and resource utilization.