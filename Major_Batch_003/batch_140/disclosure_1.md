# 12236206

## Dynamic Relevance Thresholding with Adversarial Noise Injection

**Concept:** The patent focuses on establishing relevance *before* processing. This design proposes a system where relevance isn’t a fixed pre-processing step, but dynamically adjusted *during* model training, using adversarial noise injection to force the model to be more robust to varied relevance signals.

**Specs:**

**1. System Architecture:**

*   **Core:** Existing sequence-to-sequence model (encoder/decoder) as described in patent claims.
*   **Relevance Engine:** Separate module responsible for calculating relevance scores.  This module utilizes the encoder to generate embedding vectors (as in claim 6/7) but *also* includes a ‘Noise Injector.’
*   **Noise Injector:** A configurable module that introduces random noise to the embedding vectors *before* the relevance score is calculated.  Noise type (Gaussian, Salt & Pepper, etc.) and magnitude are adjustable parameters.
*   **Dynamic Threshold:** Instead of a fixed threshold for selecting documents (claim 8), the system maintains a running average of relevance scores observed during training. This running average is then modulated by a ‘Sensitivity Parameter’ (SP).  The final selection threshold = Running Average * SP. SP is initialized to 1.0 and adjusted during training (see Training Procedure).
*   **Loss Function Modification:**  Standard loss function (comparison between first document and generated target) is augmented with a ‘Relevance Penalty’ term. If a document selected based on the dynamic threshold contributes negatively to the loss, the Relevance Penalty is applied, *and* the Sensitivity Parameter (SP) is *decreased*. If a document contributes positively, SP is *increased*. This incentivizes the model to learn more effectively from documents with varying degrees of relevance, and refines the threshold selection process.

**2. Training Procedure:**

1.  **Initialization:** Initialize the sequence-to-sequence model, Relevance Engine, Noise Injector, and SP.
2.  **Batch Selection:** For each training batch:
    *   Access first document and a plurality of second documents.
    *   Generate embedding vectors for all documents using the encoder.
    *   Inject noise into embedding vectors of second documents (Noise Injector).
    *   Calculate relevance scores for second documents.
    *   Calculate the dynamic selection threshold (Running Average * SP).
    *   Select a subset of second documents based on the threshold.
3.  **Model Training:** Generate the target document and update model parameters using backpropagation.
4.  **Threshold Adjustment:**
    *   Calculate the contribution of the selected subset of documents to the overall loss.
    *   If the contribution is negative (documents lowered loss), decrease SP by a small learning rate (e.g., 0.001).
    *   If the contribution is positive, increase SP by the same learning rate.
5.  **Repeat steps 2-4 for all training batches.**

**3. Pseudocode (Threshold Adjustment):**

```
FUNCTION adjust_threshold(selected_documents, loss_contribution):
  learning_rate = 0.001
  GLOBAL SensitivityParameter

  IF loss_contribution < 0:
    SensitivityParameter = SensitivityParameter - learning_rate
    IF SensitivityParameter < 0.1:  //Minimum threshold
      SensitivityParameter = 0.1
  ELSE:
    SensitivityParameter = SensitivityParameter + learning_rate
    IF SensitivityParameter > 2.0: //Maximum threshold
      SensitivityParameter = 2.0

  RETURN SensitivityParameter
```

**4. Novelty:**

*   **Dynamic Threshold:**  Shifts relevance selection from a static pre-processing step to an adaptive process during training.
*   **Adversarial Noise Injection:** Introduces robustness to varying degrees of relevance by exposing the model to slightly corrupted relevance signals.
*   **Sensitivity Parameter (SP):** Acts as a feedback mechanism, allowing the model to fine-tune the relevance threshold based on its performance.