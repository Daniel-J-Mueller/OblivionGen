# 10853182

## Dynamic Schema Propagation for Secondary Indexes

**Specification:** Implement a system where secondary index schemas aren't fixed at creation, but dynamically adapt to changing primary table schemas *without* requiring a full index rebuild. This tackles schema evolution in non-relational databases where such changes are common.

**Components:**

*   **Schema Change Listener:** A service monitoring primary table schema modifications (column additions, deletions, type changes).
*   **Index Schema Manager:** Responsible for maintaining the current and desired schemas for each secondary index.
*   **Delta Propagation Engine:**  The core component.  Handles schema changes by intelligently applying “delta” updates to the secondary index.
*   **Index Versioning:** Each secondary index maintains version numbers associated with the schema it reflects.

**Operation:**

1.  **Schema Change Detection:** The Schema Change Listener detects a modification to a primary table.
2.  **Impact Analysis:**  The Index Schema Manager analyzes which secondary indexes are impacted by the change.
3.  **Delta Calculation:** For each impacted index, the Delta Calculation Engine determines the minimal set of operations needed to align the index schema with the new primary table schema.  This includes:
    *   Adding new index columns.
    *   Removing obsolete index columns.
    *   Type conversions (with potential data loss warnings).
4.  **Delta Application:** The Delta Propagation Engine applies the calculated delta to the secondary index *while the database is online*.  This is achieved through a combination of:
    *   **Background Indexing:** New columns are populated in the background, utilizing change records from the log-structured journal (as in the provided patent).
    *   **Optimistic Locking/Versioning:** Index records are versioned, and updates are applied only if the version matches the expected version. This minimizes conflicts.
    *   **Atomic Swaps:** For column removals, a temporary "tombstone" flag is used, and a background process cleans up the tombstone entries.
5.  **Version Update:**  The Index Schema Manager updates the version number of the secondary index to reflect the new schema.

**Pseudocode (Delta Application):**

```
function apply_delta(index, delta):
  for each change in delta:
    if change.type == "ADD_COLUMN":
      start_sequence_number = last_processed_sequence_number(index)
      create_background_task(index, start_sequence_number, change.column_name)
    elif change.type == "REMOVE_COLUMN":
      add_tombstone_flag_to_all_records(index, change.column_name)
      schedule_cleanup_task(index, change.column_name)
    elif change.type == "TYPE_CHANGE":
      // Complex logic. Possibly requires data migration in batches.
      // Use a separate migration task.

function create_background_task(index, start_sequence_number, column_name):
  // Utilize change records from the log-structured journal
  // Populate the new column in the index records
  // Process change records starting from start_sequence_number
  // Implement error handling and retry mechanisms
```

**Storage Requirements:**

*   Index Schema Metadata (current and desired schemas, version numbers)
*   Temporary storage for background indexing tasks.

**Scalability:**

*   Distribute background indexing tasks across multiple nodes.
*   Utilize a distributed queue for managing tasks.

**Benefits:**

*   Eliminates downtime for schema evolution.
*   Reduces the cost of schema changes.
*   Improves database agility.