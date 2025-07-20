# 9691068

## Adaptive Metadata Weighting for Public Domain Determination

**Concept:** The existing patent focuses on deriving confidence levels for public domain status based on metadata. This builds on that by introducing *dynamic* weighting of metadata fields based on geographic region and historical copyright law variations. Certain metadata becomes more or less important depending on the country being analyzed.

**Specification:**

1.  **Regional Copyright Law Database:** A database containing codified copyright laws for a comprehensive list of countries, structured to allow querying for the relative importance (weight) of metadata fields. Fields include:
    *   Country Code (ISO 3166-1 alpha-2)
    *   Metadata Field (e.g., 'Author Date of Death', 'Publication Date', 'Illustrator')
    *   Weight (numerical value, 0.0 – 1.0) – indicates importance for public domain determination in that country.  This weight is not static; it can change over time as laws are updated.
    *   Law Citation (reference to specific legislation supporting the weight)
    *   Effective Date (date the weight became valid)

2.  **Metadata Extraction & Normalization Module:** (Existing functionality – assumed from the base patent) Extracts metadata from network resources, normalizes data types (dates, names, etc.).

3.  **Dynamic Weight Assignment Module:**
    *   Input: Extracted metadata, Target Country Code.
    *   Process:
        *   Query the Regional Copyright Law Database for weights associated with each metadata field for the Target Country Code.  If a field is not found, assign a default weight (e.g., 0.1).
        *   Apply the weights to the corresponding metadata values.  Values should be normalized to a consistent scale (e.g., 0.0 – 1.0).  For date-based fields, calculate the time elapsed since the date.

4.  **Confidence Level Calculation Module:**
    *   Input: Weighted metadata values.
    *   Process:
        *   Calculate a weighted sum of the metadata values to generate a confidence level.
        *   The confidence level is expressed as a numerical value between 0.0 (definitely copyrighted) and 1.0 (definitely public domain).
        *   Formula: `Confidence Level = Σ (Weighted Metadata Valueᵢ)` , where `i` iterates through all metadata fields.

5.  **Threshold Adjustment Module:**
    *   Country-specific thresholds are maintained for determining public domain status based on the Confidence Level. These thresholds can be adjusted over time based on legal precedent and expert review.

6.  **User Interface Integration:** The UI displays the Confidence Level, the contributing metadata fields and their weights, and the recommended public domain status.  Users can drill down into the specific legal basis for the weights.

**Pseudocode:**

```
FUNCTION DeterminePublicDomainStatus(work_info, country_code):
  metadata = ExtractMetadata(work_info)
  weights = GetCopyrightWeights(country_code) // Query database
  confidence_level = 0

  FOR field IN metadata:
    IF field IN weights:
      weighted_value = metadata[field] * weights[field]
      confidence_level += weighted_value
    ELSE:
      // Apply default weight
      weighted_value = metadata[field] * 0.1
      confidence_level += weighted_value

  threshold = GetCountryThreshold(country_code)

  IF confidence_level >= threshold:
    status = "Public Domain"
  ELSE:
    status = "Copyrighted"

  RETURN status, confidence_level, metadata, weights
```

**Potential Expansion:** Incorporate Machine Learning to refine the weights based on a dataset of known public domain works and copyrighted works. This would allow the system to adapt to nuanced legal interpretations and edge cases.