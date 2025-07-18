# 11475067

## Adaptive Query Expansion via Simulated User Intent Drift

**Concept:** The existing patent focuses on generating training data from private documents. This builds on that by *actively* modulating the generated questions to simulate shifts in user intent over time, enhancing the robustness of the trained models. Instead of static Q&A pairs, we create a dynamic training set reflecting evolving information needs.

**Specs:**

**Module Name:** Intent Drift Simulator (IDS)

**Inputs:**

*   Private Document Set (as in the existing patent)
*   Time Series Data: Representing external knowledge shifts (e.g., news trends, product updates, seasonal changes – data source external to the system, accessed via API).  Format: JSON array of events, each with timestamp, keywords, and a 'drift_magnitude' score (0.0 – 1.0).
*   Initial Question/Answer Pair Generation Model (the model described in the patent).
*   Hyperparameter: *Drift Sensitivity* (float, 0.0-1.0) – Controls how strongly the time series data influences question generation.

**Outputs:**

*   Augmented Question/Answer Pair Dataset.

**Process:**

1.  **Baseline Q&A Generation:** Generate an initial set of Q&A pairs from the Private Document Set using the existing patent’s method.

2.  **Time Series Analysis:** Analyze the Time Series Data to identify dominant keywords and their associated ‘drift_magnitude’ over defined time windows (e.g., weekly, monthly).

3.  **Intent Vector Creation:** For each time window, create an ‘Intent Vector’ representing the weighted average of keywords based on their drift magnitude.  Higher drift magnitude = stronger weight.

4.  **Question Perturbation:**  For *each* question in the baseline Q&A set:
    *   Calculate a ‘Perturbation Score’ based on the dot product of the question’s keyword vector (obtained via NLP) and the current time window’s Intent Vector.
    *   Apply controlled perturbations to the question based on the Perturbation Score:
        *   **Keyword Insertion:**  Randomly insert keywords from the Intent Vector into the question (probability proportional to Perturbation Score).
        *   **Keyword Replacement:** Replace existing keywords in the question with keywords from the Intent Vector (probability proportional to Perturbation Score).
        *   **Question Rephrasing:**  Utilize a paraphrasing model (trained separately) to subtly rephrase the question, incorporating keywords from the Intent Vector.  The degree of rephrasing is controlled by the Perturbation Score.
        *   **Drift Sensitivity Parameter**: The "Drift Sensitivity" parameter controls the magnitude and frequency of these perturbations.

5.  **Answer Validation:** After perturbing the question, *verify* that the answer remains valid.  This is crucial. Use a dedicated Answer Validity Model (trained separately) to assess whether the answer still accurately addresses the modified question. Discard any question/answer pairs where the answer is deemed invalid.

6.  **Dataset Augmentation:** Append the augmented Q&A pairs to the baseline dataset.

7.  **Iteration:** Repeat steps 2-6 for multiple time windows, creating a time-series-aware Q&A dataset.

**Pseudocode:**

```python
def augment_qa_dataset(private_docs, time_series_data, drift_sensitivity):
  baseline_qa = generate_qa_pairs(private_docs) # Existing patent's method
  augmented_qa = []

  for time_window in time_series_data:
    intent_vector = analyze_time_series(time_window)
    for question, answer in baseline_qa:
      perturbation_score = dot_product(question_keywords, intent_vector)
      perturbed_question = perturb_question(question, intent_vector, perturbation_score, drift_sensitivity)
      if is_answer_valid(perturbed_question, answer):
        augmented_qa.append((perturbed_question, answer))

  return augmented_qa
```

**Model Training Requirements:**

*   Answer Validity Model (binary classification – valid/invalid).
*   Paraphrasing Model (text-to-text transformation).

**Potential Benefits:**

*   More robust document querying models.
*   Improved ability to handle evolving user information needs.
*   Greater adaptability to changing knowledge landscapes.