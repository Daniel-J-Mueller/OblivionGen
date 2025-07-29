# 6760470

## Automated Check Image Enhancement & Data Extraction for Mobile Deposit

**Concept:** Extend the MICR line reading capability to *any* check image captured via a mobile device camera, even in suboptimal lighting or with perspective distortion. This goes beyond simply reading the MICR line; it reconstructs a usable data set from a potentially flawed image.

**Specs:**

**1. Image Pre-processing Module:**

*   **Input:** Raw check image (JPEG, PNG) captured via mobile device camera.
*   **Steps:**
    *   **Perspective Correction:** Employ a homography matrix calculation to correct for perspective distortion caused by the camera angle.  Feature detection (e.g., corners of the check) is used to establish the transformation.
    *   **Lighting Normalization:** Utilize histogram equalization and/or adaptive histogram equalization (CLAHE) to improve contrast and compensate for uneven lighting.
    *   **Noise Reduction:** Apply a bilateral filter or non-local means denoising to reduce noise while preserving edges.
    *   **Binarization:** Convert the image to black and white using an adaptive thresholding algorithm (e.g., Otsu's method) to separate the printed characters from the background.

**2. Character Segmentation & Recognition Module:**

*   **Input:** Processed binary image.
*   **Steps:**
    *   **Connected Component Analysis:** Identify individual characters as connected components.
    *   **Character Isolation:**  Handle touching or overlapping characters using techniques like profile analysis or contour splitting.
    *   **Optical Character Recognition (OCR):** Utilize a specialized OCR engine trained on MICR E-13B fonts and common check fonts. The engine *must* include confidence scoring for each character.
    *   **Contextual Validation:**  Implement rules based on the expected format of check data (e.g., routing number, account number, check number). Discard characters or segments with low confidence scores or those that violate the expected format.

**3. Data Reconstruction & Verification Module:**

*   **Input:** Recognized characters and confidence scores.
*   **Steps:**
    *   **Data Assembly:** Assemble the recognized characters into potential routing numbers, account numbers, and check numbers based on the expected positions on a standard check.
    *   **Checksum Validation:** Verify the validity of the assembled routing number using the standard checksum algorithm.
    *   **MICR Line Reconstruction:**  Employ a Hidden Markov Model (HMM) trained on known MICR line patterns to reconstruct the most likely MICR line sequence from the recognized characters, even if some characters are missing or misrecognized. The HMM incorporates context from the expected structure of the MICR line.
    *   **Account Number & Check Number Validation**: Use known patterns & context to extract potential account & check numbers.
    *   **Confidence Score Aggregation**: Return the extracted data with an aggregate confidence score reflecting the reliability of the extracted information.

**4. Mobile SDK Integration:**

*   Provide a lightweight mobile SDK (iOS and Android) that integrates seamlessly into existing mobile banking applications.
*   The SDK should handle camera access, image capture, pre-processing, and data extraction.
*   The SDK should provide APIs for accessing the extracted data and confidence scores.
*   SDK should provide user feedback mechanisms (e.g., highlighting extracted fields, displaying confidence scores, prompting for re-capture if necessary).



**Pseudocode (Data Extraction Pipeline):**

```
function extractCheckData(image):
  processedImage = preprocessImage(image)
  candidateData = segmentAndRecognize(processedImage)
  validatedData = validateAndReconstruct(candidateData)
  return validatedData
```

**Novelty:**

This system goes beyond simply reading the MICR line. It actively reconstructs the data from potentially poor-quality images, making mobile check deposit significantly more reliable, even in challenging environments. The HMM-based reconstruction and confidence scoring provide a robust and accurate data extraction pipeline. It’s not about ‘better OCR,’ but about building an entire *system* capable of handling degraded input.