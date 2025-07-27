# 11372811

## Dynamic Snapshot Provenance & Predictive Threat Hunting

**Concept:** Extend snapshot metadata to include a provenance graph, tracking not just *what* changed between snapshots, but *how* those changes occurred (user, application, system process). Couple this with a predictive threat model that learns normal change patterns and flags anomalies *before* a full scan, focusing resources on likely threat vectors.

**Specs:**

*   **Provenance Capture Service:**
    *   Integrates with existing snapshotting service.
    *   Records metadata associated with each block change:
        *   Timestamp
        *   User ID (if applicable)
        *   Application/Process ID initiating the change
        *   File Path (of the affected file/directory)
        *   Change Type (create, modify, delete)
        *   System Call Signature (if applicable - capturing the exact API call triggering the change)
    *   Stores provenance data as a directed acyclic graph (DAG) – allowing tracing of changes back through multiple snapshots.  Graph database (e.g., Neo4j) recommended for storage and efficient traversal.
*   **Behavioral Baseline Engine:**
    *   Analyzes provenance graphs across multiple storage volumes and users to establish baseline change patterns:
        *   Typical applications used for file modifications.
        *   Expected file modification frequencies.
        *   Normal user behavior (e.g., time of day, file types accessed).
        *   Typical system call sequences.
    *   Employs machine learning (e.g., anomaly detection algorithms, time series analysis) to model normal behavior.
*   **Predictive Threat Scoring System:**
    *   Real-time analysis of incoming snapshot change data.
    *   Scores changes based on deviation from established baselines. Factors to consider:
        *   Unusual application/process initiating the change.
        *   Unexpected file modification frequency.
        *   Rare or suspicious system call sequences.
        *   User activity outside of normal patterns.
    *   Assigns a threat score to each snapshot *before* scanning.
*   **Dynamic Scan Prioritization:**
    *   Scanning service utilizes threat scores to prioritize scanning efforts.
    *   High-score snapshots undergo full scans.
    *   Low-score snapshots may be skipped or undergo limited/targeted scans.
    *   Allows for defining customizable scan policies based on threat scores.
* **Data Structure – Provenance Block Metadata:**
    ```
    {
        "block_id": "unique_block_identifier",
        "snapshot_id": "snapshot_identifier",
        "timestamp": "ISO8601 timestamp",
        "user_id": "user_identifier (optional)",
        "process_id": "process_identifier",
        "application_name": "application_name",
        "change_type": "create/modify/delete",
        "file_path": "/path/to/file",
        "syscall_signature": "string (syscall details)",
        "parent_block_id": "block_id of prior version (if applicable)"
    }
    ```

**Pseudocode – Threat Score Calculation:**

```
function calculateThreatScore(blockMetadata):
  score = 0

  # Application/Process Reputation
  if isKnownMaliciousProcess(blockMetadata.process_id):
    score += 50

  # Frequency Anomaly
  fileModificationFrequency = getFileModificationFrequency(blockMetadata.file_path)
  if blockMetadata.timestamp - fileModificationFrequency > threshold:
    score += 20

  # Syscall Anomaly
  if isSuspiciousSyscallSequence(blockMetadata.syscall_signature):
    score += 30

  # User Behavior Anomaly
  if isOutsideNormalUserHours(blockMetadata.timestamp, blockMetadata.user_id):
    score += 10

  return score
```