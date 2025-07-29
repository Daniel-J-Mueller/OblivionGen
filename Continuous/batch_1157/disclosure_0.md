# 10409917

**Dynamic Proposition Weighting based on Semantic Role Labeling**

**Specification:**

1.  **System Overview:** Extend the existing proposition comparison framework with a semantic role labeling (SRL) module. SRL identifies the arguments associated with predicates (verbs/nouns) within both the source and target language strings.

2.  **Proposition Identification & SRL Integration:**
    *   Existing proposition extraction module operates as before.
    *   For each identified proposition in both source and target, apply an SRL parser. This parser outputs a set of arguments (Agent, Patient, Instrument, Location, etc.) associated with the main predicate of the proposition.

3.  **Dynamic Weight Assignment:**
    *   Assign initial weights to each proposition based on its presence/absence in the target.
    *   **Crucially:** Analyze the SRL outputs. If an argument is *missing* or *incorrectly* mapped in the target language proposition, *increase* the penalty applied to that proposition’s weight. The magnitude of the increase is dependent on the *semantic role* of the missing/incorrect argument.
    *   **Rationale:** Losing the Agent of an action is more significant than losing a Location modifier. Implement a lookup table mapping semantic roles to penalty multipliers (e.g., Agent: x3, Patient: x2, Location: x0.5).
    *   **Example:** Source: "John kicked the ball." Target: "The ball was kicked."
        *   Original proposition weight: -1 (missing Agent ‘John’).
        *   SRL analysis: Agent (John) is missing.
        *   Penalty multiplier (Agent): 3.
        *   Adjusted weight: -1 * 3 = -3.

4.  **Quality Score Calculation:**
    *   Modify the quality score formula to incorporate dynamically weighted propositions.
    *   New Formula: `Quality Score = (Sum of Correctly Mapped Proposition Weights) / (Total Number of Propositions)`.

5.  **Training & Optimization:**
    *   Create a labeled dataset of translation pairs with varying degrees of semantic accuracy.
    *   Train a machine learning model (e.g., a regression model) to optimize the penalty multipliers for different semantic roles based on the training data. This ensures the weighting system accurately reflects human judgments of translation quality.

6.  **Implementation Details:**
    *   Utilize a pre-trained SRL parser (e.g., AllenNLP SRL, SpaCy with SRL extension).
    *   Implement a flexible weighting system allowing for adjustments based on language pair or domain.

**Pseudocode:**

```
function calculate_quality_score(source_string, target_string):
  source_propositions = extract_propositions(source_string)
  target_propositions = extract_propositions(target_string)

  total_score = 0
  total_propositions = len(source_propositions)

  for source_prop in source_propositions:
    target_prop = find_matching_proposition(source_prop, target_propositions)

    if target_prop:
      weight = calculate_proposition_weight(source_prop, target_prop)
      total_score += weight
    else:
      total_score -= 2 # significant penalty for missing proposition

  quality_score = total_score / total_propositions
  return quality_score

function calculate_proposition_weight(source_prop, target_prop):
  # Initial weight (1 if matched, 0 if not)
  weight = 1

  source_srl = perform_srl(source_prop)
  target_srl = perform_srl(target_prop)

  for role, argument in source_srl.items():
    if role not in target_srl or target_srl[role] != argument:
      penalty_multiplier = get_penalty_multiplier(role)
      weight -= penalty_multiplier
  return weight
```