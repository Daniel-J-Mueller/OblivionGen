# 11036762

## Dynamic Key Dimensionality & Fractal Indexing

**Concept:** Extend the compound key concept beyond simple concatenation of values by introducing *dimensionality* to the keys themselves, and leveraging fractal indexing for extremely efficient storage and retrieval, particularly with variable-length data.

**Inspiration:** The patent's focus on manipulating key lengths and chunking.  This takes that idea to an extreme, making key dimensionality a first-class concept.

**Specs:**

**1. Key Dimensionality Definition:**

*   Each key value (partition or clustering) is not treated as a flat byte string. Instead, it’s defined as a multi-dimensional array/tensor.
*   Dimensions represent *features* or *attributes* of the key.  Example: a user ID key might have dimensions:  `[user_id, last_login_timestamp, subscription_tier]`.
*   Each dimension can have a variable length (e.g., subscription tier could be a single byte, last login could be a timestamp requiring 8 bytes).
*   Metadata associated with each key value specifies the number of dimensions and the length of each dimension. This metadata is crucial for reconstructing the key.

**2. Fractal Indexing Scheme:**

*   Instead of a traditional tree-based index, employ a fractal indexing scheme (e.g., a space-filling curve like a Hilbert curve or a Z-order curve).
*   The multi-dimensional key (flattened for indexing) is mapped onto the fractal curve.  The curve’s position represents the data’s storage location.
*   The fractal curve provides inherent locality of reference.  Similar keys (in terms of their multi-dimensional feature space) will be stored physically close to each other.

**3. Dynamic Key Adaptation:**

*   The system dynamically adjusts the dimensionality of keys based on data access patterns and schema evolution.
*   If a new attribute becomes important for filtering or sorting, a new dimension is added to the key.
*   The fractal index is updated incrementally to reflect the new dimensionality.  This is critical for maintaining performance.
*   Old dimensions can be removed if they are no longer used.

**4. Data Storage:**

*   Data is stored as blobs associated with positions on the fractal curve.
*   Blobs include the data itself and metadata indicating the key dimensionality and lengths.

**5. Access Pattern:**

*   Access requests specify a subset of key dimensions (e.g., find all users with `subscription_tier = premium`).
*   The system uses the specified dimensions to narrow down the search space on the fractal curve.
*   Filtering is performed efficiently by traversing the curve and comparing the specified dimensions.

**Pseudocode (Simplified Access):**

```
function accessData(keyDimensions, filterDimensions):
  // keyDimensions: Dictionary of key dimensions and values
  // filterDimensions: Dictionary of dimensions to filter on

  fractalPosition = mapKeyToFractalPosition(keyDimensions)

  // Find the starting position on the fractal curve
  startPosition = findStartPosition(fractalPosition)

  // Iterate through the fractal curve starting at startPosition
  for each position in fractalCurve(startPosition):
    data = getDataAtPosition(position)
    // Check if data matches the filter criteria
    if matchesFilter(data, filterDimensions):
      return data

  return null // Data not found
```

**Benefits:**

*   **Scalability:** Fractal indexing provides excellent scalability for large datasets.
*   **Flexibility:** Dynamic key adaptation allows the system to evolve with changing data requirements.
*   **Performance:** Locality of reference and efficient filtering enable fast access to data.
*   **Dimensionality Reduction:** Unused dimensions can be eliminated reducing overhead.
*   **Multi-dimensional Search:** Efficiently support queries based on multiple key attributes.