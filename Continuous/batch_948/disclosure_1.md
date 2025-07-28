# 11113254

## Dynamic Blocking Key Mutation for Enhanced Record Linkage Robustness

**Concept:** Extend the dynamic blocking process with a mutation step applied to blocking keys. This introduces controlled noise into the key space, preventing premature convergence and improving the identification of near-match records that might otherwise be missed due to slight variations in data entry or formatting.

**Specification:**

**1. Mutation Profiles:** Define a set of mutation profiles. Each profile consists of a set of transformation functions applicable to different data types within blocking keys. 

   *   **String Mutations:**
        *   *Transposition:* Swap adjacent characters (e.g., “Smith” -> “Smith”). Probability controlled per profile.
        *   *Deletion:* Remove a character with a defined probability.
        *   *Insertion:* Insert a character (potentially a common error) with a defined probability.
        *   *Substitution:* Replace a character with another (e.g. 'o' with '0').
        *   *Phonetic Encoding:* Apply a phonetic algorithm (Soundex, Metaphone) to generate a phonetic representation of the key.
   *   **Numeric Mutations:**
        *   *Rounding:* Round the number to a specified number of decimal places.
        *   *Truncation:* Truncate the number after a specific number of decimal places.
        *   *Range Shift:* Add or subtract a small random value from the number.
   *   **Date Mutations:**
        *   *Day/Month Swap:* Swap the day and month components.
        *   *Year Offset:* Add or subtract a small number of years.

**2. Mutation Application:** After each dynamic blocking iteration, apply mutation to the blocking keys of the surviving blocks.

   *   For each block, randomly select a mutation profile.
   *   For each key within the block, apply the transformations defined in the selected profile with their respective probabilities.
   *   Create mutated blocks using the transformed keys.
   *   Merge the mutated blocks with the original blocks.
   *   Re-evaluate block similarity and prune overlapping blocks as in the original dynamic blocking algorithm.

**3. Adaptive Mutation:** Dynamically adjust mutation parameters based on data characteristics and blocking progress.

   *   **Data Type Aware Mutation:** Apply different mutation profiles based on the data type of the blocking key.
   *   **Blocking Progress Adaptive Mutation:**
        *   Increase the mutation rate (probability of applying transformations) as blocking progresses, allowing for broader exploration of the key space.
        *   Decrease the mutation rate as blocking converges, refining the search and reducing noise.
   *   **Error Signature Detection:** Track common data entry errors based on mutations that frequently lead to matches. Prioritize those mutations in subsequent iterations.

**Pseudocode:**

```
function dynamicBlockingWithMutation(records, blockingParameters, maxIterations):
  blocks = initialBlocking(records, blockingParameters)
  for iteration in range(maxIterations):
    # Prune overlapping blocks based on similarity
    blocks = pruneOverlappingBlocks(blocks)

    # Apply mutation to blocks
    mutatedBlocks = []
    for block in blocks:
      mutationProfile = selectMutationProfile(block)
      mutatedBlock = applyMutation(block, mutationProfile)
      mutatedBlocks.append(mutatedBlock)
    
    blocks.extend(mutatedBlocks)

    #Prune overlapping blocks again
    blocks = pruneOverlappingBlocks(blocks)

    if len(blocks) < thresholdSize:
      break
  
  return blocks
```

**Data Structures:**

*   `MutationProfile`: A dictionary mapping data types to lists of transformation functions and associated probabilities.
*   `Block`: A data structure containing a list of records and the corresponding blocking keys.
*   `SimilarityMetric`: A function that calculates the similarity between two blocks.