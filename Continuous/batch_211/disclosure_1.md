# 12182498

**Adaptive Redaction Granularity based on Semantic Role Labeling**

**Concept:** Expand the redaction capabilities beyond simply identifying *what* to redact, to *how much* of the surrounding context should also be redacted, dynamically adjusting redaction granularity based on semantic importance.

**Specification:**

1.  **Semantic Role Labeling (SRL) Integration:**  Implement an SRL module that analyzes the normalized text data *before* redaction.  SRL identifies the semantic roles of words/phrases (Agent, Patient, Instrument, Location, Time, Manner, etc.). This goes beyond Named Entity Recognition (NER) which just identifies *types* of entities.

2.  **Redaction Granularity Score (RGS):** Develop an RGS algorithm. The RGS considers:
    *   The type of redacted entity (e.g., PII, sensitive keyword).
    *   The SRL roles of the redacted entity.  (e.g., If "John Smith" is the Agent in an action, the action itself is more likely to be sensitive).
    *   Distance to other redacted entities. (Clustering of sensitive information increases the need for broader redaction.)
    *   User-defined policy thresholds (e.g.  Redact everything within +/- 5 words of a PII entity with high confidence.)

3.  **Dynamic Redaction Mask:** Based on the RGS, generate a dynamic redaction mask. The mask specifies *how much* of the surrounding text to redact, not just the identified entity.  This isn't binary (redact/don't redact), but a sliding scale of redaction levels (e.g. full redaction, partial masking with asterisks, semantic anonymization replacing words with synonyms).

4.  **Anonymization Options:**  Beyond simply replacing with "[REDACTED]", include:
    *   **Semantic Replacement:** Replace with a semantically similar, non-identifying term (requires a large knowledge graph/ontology).  Example: "visited New York" -> "visited a major city".
    *   **Generalized Location:** Replace specific locations with broader categories (e.g., "123 Main Street, Anytown" -> "a residential address").
    *   **Temporal Generalization:**  Replace specific times with ranges or broader time periods.

5.  **Pseudocode for Redaction Granularity Calculation:**

```
FUNCTION CalculateRedactionGranularity(entity, entityType, confidence, surroundingText, policyThreshold)

    roles = SemanticRoleLabeling(surroundingText)
    granularityScore = 0

    IF entityType == "PII" AND confidence > 0.9 THEN
        granularityScore += 5  // High confidence PII gets a base score
    ENDIF

    FOR role IN roles
        IF role.entity == entity AND role.type == "Agent" THEN
            granularityScore += 3
        ELSIF role.entity == entity AND role.type == "Patient" THEN
            granularityScore += 2
        ENDIF
    ENDFOR

    IF proximityToOtherRedactedEntity(surroundingText) < 5 words THEN
        granularityScore += 4
    ENDIF

    IF granularityScore > policyThreshold THEN
        redactionGranularity = "Full Redaction"
    ELSE
        redactionGranularity = "Partial Masking"
    ENDIF

    RETURN redactionGranularity

END FUNCTION
```

6.  **Integration Points:**  This system integrates after the inverse text normalization and *before* final output. The machine learning model for redaction would provide the initial entities and confidence scores.

7. **Adaptive Policy Framework**: Allow administrators to define granular policies based on entity types, semantic roles, and the sensitivity of the data. This framework enables dynamic adjustment of redaction rules without modifying the core algorithm.