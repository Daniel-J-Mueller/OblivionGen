# 10839135

## Dynamic Watermarking via Stylometric Drift

**Concept:** Extend the text modification approach of the provided patent to include subtle, stylometric drifts – alterations to writing *style* – rather than purely semantic or syntactic changes. This allows for a higher degree of obfuscation and potentially increased robustness against simple text alteration attacks. The core idea is to embed a unique identifier not through direct encoding, but by subtly shifting the distribution of stylistic features within the text.

**Specs:**

**1. Stylometric Feature Extraction Module:**

*   **Input:** Raw text.
*   **Process:** Analyze text for a defined set of stylometric features. These include:
    *   Average Sentence Length
    *   Word Frequency Distribution (top N words)
    *   Vocabulary Richness (Type-Token Ratio)
    *   Distribution of Parts-of-Speech (POS tags)
    *   Readability Scores (Flesch-Kincaid, SMOG, etc.)
    *   Use of specific function words (e.g., prepositions, articles)
    *   N-gram frequencies (character and word-level)
*   **Output:** Vector representing the statistical distribution of the identified stylometric features.

**2. Identifier-to-Stylometric-Shift Mapping:**

*   **Input:** Unique identifier, baseline stylometric feature vector (derived from a “neutral” text corpus).
*   **Process:**  Employ a deterministic algorithm to translate the identifier into a set of desired shifts in the stylometric feature vector.  This could involve:
    *   Scaling factors applied to specific feature values (e.g., increase average sentence length by X%).
    *   Target distributions for feature values (e.g., shift POS tag distribution to favor passive voice).
    *   A look-up table mapping identifier segments to specific feature adjustments.
*   **Output:**  A delta vector representing the desired change in stylometric features.

**3. Text Modification Engine:**

*   **Input:** Raw text, delta vector.
*   **Process:**  Modify the input text to approximate the target stylometric feature distribution represented by the delta vector.  This is the most complex stage and could involve:
    *   **Paraphrasing:**  Replace phrases with synonyms or alternative constructions to alter sentence structure and vocabulary.
    *   **Sentence Splitting/Combining:**  Break long sentences into shorter ones, or combine short sentences to adjust average sentence length.
    *   **Word Choice Adjustment:** Select words with subtly different connotations or stylistic properties.
    *   **Insertion/Deletion of Modifier Words:** Add or remove adverbs, adjectives, and other modifying elements.
*   **Output:** Modified text with embedded stylistic watermark.

**4. Detection & Verification Module:**

*   **Input:** Potentially compromised text.
*   **Process:**
    1.  Extract stylometric features from the compromised text.
    2.  Compare the extracted features to a known baseline and/or expected watermark profile.
    3.  If a significant deviation is detected, attempt to reconstruct the embedded identifier based on the observed stylometric shifts.
    4.  Compare the reconstructed identifier to the expected identifier.
*   **Output:**  Binary indicator (valid/invalid) and potentially the reconstructed identifier.

**Pseudocode (Detection):**

```
function detect_watermark(text, expected_identifier):
  features = extract_stylometric_features(text)
  baseline_features = extract_stylometric_features(neutral_text)
  deviation = calculate_stylometric_deviation(features, baseline_features)

  if deviation exceeds threshold:
    reconstructed_identifier = reconstruct_identifier_from_deviation(deviation)
    if reconstructed_identifier == expected_identifier:
      return True, reconstructed_identifier
    else:
      return False, reconstructed_identifier
  else:
    return False, null
```

**Novelty:** This approach shifts the focus from direct text manipulation to subtle stylistic adjustments, making the watermark more difficult to detect and remove through simple text editing or paraphrasing. The use of a deterministic algorithm ensures consistent encoding and decoding, and allows for a potentially large number of unique identifiers. The reliance on statistical distributions makes the watermark more robust to minor variations in the text.