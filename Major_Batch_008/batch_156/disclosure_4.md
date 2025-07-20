# 11250552

## Adaptive Defect Injection & Simulated Reviewer Drift

**Concept:** Expand the training data augmentation strategy beyond static noise to *simulate* evolving human reviewer standards. This leverages the core idea of incomplete labels but adds a dynamic element reflecting real-world changes in quality control.

**Specs:**

*   **Module:** “Reviewer Drift Simulator” – a data augmentation module integrated into the training pipeline.
*   **Input:** Original image dataset with initial human review labels (positive/negative/unknown), and a ‘drift parameter’ (float between 0.0 and 1.0).
*   **Process:**
    1.  **Base Probability Adjustment:**  Each image undergoes a base probability adjustment based on the ‘drift parameter’.  Higher drift values mean greater probability of label *changes*.
    2.  **Label Swapping:** For images initially labeled 'accepted' (all negatives), a portion are re-labeled as 'rejected' with a defect introduced (see below). The probability of this swap is proportional to the drift parameter.
    3.  **Defect Injection:** When an image is re-labeled as rejected, a defect is injected. Defect injection uses a procedural generation system.
        *   **Defect Library:** A library of simulated defects (scratches, stains, discoloration, etc.) is maintained.
        *   **Procedural Generation Parameters:** Each defect type has associated procedural generation parameters (size, intensity, location variation). These are randomly varied within defined ranges.
        *   **Realistic Blending:** Blending algorithms are applied to ensure the injected defect appears realistic within the image.
    4.  **Unknown Label Introduction:** For a portion of images initially labeled with only positive defects, additional defects are *removed* (defect masking). This creates images labeled with unknown defects, simulating scenarios where reviewers miss defects. Probability of this is proportional to drift.
    5.  **Drift Scheduling:**  The ‘drift parameter’ isn’t constant. It can be scheduled to increase over training epochs, simulating a gradual shift in reviewer standards over time.  This mimics the real-world scenario where QC expectations change.
*   **Output:** Augmented dataset with modified labels and injected/masked defects.

**Pseudocode:**

```python
def augment_data(image, label, drift_parameter, defect_library, epoch):
    augmented_image = image.copy()
    augmented_label = label.copy()

    # Randomly determine if augmentation should occur
    if random.random() < drift_parameter:

        if label == "accepted": # all negatives
            # Re-label as rejected and inject defect
            augmented_label = "rejected"
            defect_type = random.choice(defect_library.keys())
            defect_parameters = defect_library[defect_type]
            augmented_image = inject_defect(augmented_image, defect_type, defect_parameters)

        elif label == "rejected": # positive defects
            # Remove a defect and mark as unknown
            defect_to_remove = random.choice(positive_defect_list) #list of defects already present
            augmented_image = mask_defect(augmented_image, defect_to_remove)
            augmented_label["unknown"] = True #mark defect as unknown.

    return augmented_image, augmented_label
```

**Hardware/Software Considerations:**

*   GPU acceleration for defect injection/masking.
*   Procedural generation engine for creating diverse defects.
*   Data pipeline capable of handling large datasets.
*   Integration with existing machine learning training framework.