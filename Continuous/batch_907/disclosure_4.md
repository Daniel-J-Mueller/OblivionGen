# 11222036

## Dynamic Data Masking via Query Plan Augmentation

**Concept:** Extend the auditing framework to *dynamically mask* sensitive data within query results *before* they are returned to the user, triggered by the same auditing metadata flag. This shifts from merely logging access to actively controlling data exposure.

**Specs:**

1.  **Metadata Extension:** The existing `first column` (audit flag) must gain an additional attribute: `masking_level`. This level can be:
    *   `0`: No masking (default).
    *   `1`: Redact – Replace sensitive data with a placeholder (e.g., “XXX-XXX-XXXX” for a social security number).
    *   `2`: Tokenize – Substitute sensitive data with a randomly generated token. The token-to-value mapping is stored securely.
    *   `3`: Encrypt – Encrypt the sensitive data using a key accessible only to authorized applications.

2.  **Query Plan Augmentation Module:**  Introduce a new module within the query optimizer. This module activates when:
    *   The query accesses a table flagged for auditing (via the `first column`).
    *   The `masking_level` is greater than 0.

3.  **Dynamic Masking Logic (within Query Plan):**
    *   The query plan will be modified to include steps for masking the appropriate columns.
    *   The specific masking function (redaction, tokenization, encryption) is determined by the `masking_level`.
    *   For tokenization:
        *   A token cache will be used to minimize performance impact.
        *   The cache should have an expiration policy.
        *   The mapping between tokens and values must be auditable.
    *   For encryption:
        *   The appropriate encryption key is retrieved securely.

4.  **Pseudocode (Query Plan Augmentation):**

```
function augment_query_plan(query_plan, table_metadata):
  for step in query_plan:
    if step.table == table_metadata.table_name and table_metadata.audit_flag:
      masking_level = table_metadata.masking_level
      if masking_level > 0:
        for column in step.columns:
          if column in table_metadata.sensitive_columns:
            if masking_level == 1:
              add_redaction_step(step, column)
            else if masking_level == 2:
              add_tokenization_step(step, column)
            else if masking_level == 3:
              add_encryption_step(step, column)
  return query_plan
```

5. **Tokenization Cache:**

    *   Key: Combination of table name, column name, and original value.
    *   Value:  Generated token.
    *   Expiration: Configurable, based on sensitivity level.
    *   Secure Storage: Encrypted at rest and in transit.

6.  **Auditing Integration:**  The audit log should include information about the masking applied, including the masking level and the method used.

**Novelty:** This isn't simply about *logging* access but proactively controlling data exposure during query execution.  The dynamic nature, driven by query plan augmentation, allows for fine-grained control without requiring application-level changes.  The addition of masking levels provides a tiered approach to data protection.