# 11430102

## Dynamic Watermarking via Perceptual Hash Drift

**Concept:** Implement a dynamic watermarking system that doesn't embed a static watermark, but instead subtly alters perceptual hashes of images over time, creating a "drift" pattern unique to the image’s history. This allows for authentication *and* provenance tracking without visible watermarks.

**Specs:**

1.  **Perceptual Hash Generator:**  A robust perceptual hash algorithm (pHash) optimized for resilience to common image manipulations (compression, resizing, color adjustments). Key is speed *and* consistency across processing pipelines.
2.  **Drift Vector:**  A small, pseudo-random vector (e.g., 3-5 floating-point values). This vector defines the direction and magnitude of hash "drift" for a given image. The vector is derived from a combination of:
    *   Image content (using features extracted from a convolutional neural network - CNN).
    *   Timestamp (for chronological tracking).
    *   Seed value (unique to the image upon initial creation/upload).
3.  **Hash Modification Function:**  A function that subtly modifies the pHash.  Instead of directly embedding data, the function adds a weighted version of the Drift Vector to the pHash components. Weights should be dynamically adjusted based on image content to maintain imperceptibility.
4.  **Provenance Database:** A database storing:
    *   Original image hash.
    *   List of modified hashes with associated timestamps and modification parameters (Drift Vector, weights).
    *   Metadata about the modification process (user ID, application used).
5.  **Verification Algorithm:**  A process to verify image authenticity and reconstruct the provenance history:
    *   Calculate the current pHash of the image.
    *   Compare it to the last known pHash in the database.
    *   If a difference exists, attempt to reconstruct the intervening hashes using the stored modification parameters.
    *   If reconstruction is successful, the image is considered authentic and the provenance history is verified.
6. **Adaptive Drift Rate:**  Implement a system to adjust the rate of hash drift based on the image’s predicted lifespan or usage.  Images expected to have a short lifespan (e.g., social media posts) can have a faster drift rate, while archival images can have a slower rate. This balances authentication accuracy with the need to avoid excessive data storage for long-lived images.

**Pseudocode (Verification Algorithm):**

```
function verifyImage(image, database):
  currentHash = calculatePerceptualHash(image)
  lastKnownHash = database.getLastHash(image)

  if currentHash == lastKnownHash:
    return True, "Image unchanged"

  reconstructedHashes = []
  success = True
  
  for i in range(database.getNumberOfModifications(image)):
    modificationParameters = database.getModificationParameters(image, i)
    previousHash = (i == 0) ? database.getOriginalHash(image) : reconstructedHashes[-1]
    newHash = modifyHash(previousHash, modificationParameters)
    reconstructedHashes.append(newHash)
    
    if abs(newHash - currentHash) > threshold:
      success = False
      break
      
  if success:
    database.updateLastHash(image, currentHash)
    return True, "Image authenticated. Provenance verified."
  else:
    return False, "Image failed authentication or provenance verification."
```

**Novelty:** This system moves beyond static watermarks by embedding a dynamic "fingerprint" into the image’s perceptual hash. The drift pattern provides both authentication *and* a traceable history of modifications, without relying on visible watermarks. The adaptive drift rate optimizes performance for different types of images.