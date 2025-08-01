# 10146877

## Dynamic Subspace Merging & Splitting

**Concept:** Extend the subspace system with the ability for subspaces to dynamically merge or split based on object density, user interaction, or computational load. This introduces a hierarchical, adaptive virtual space organization.

**Specs:**

*   **Subspace Class:**  Extend the existing subspace class to include attributes: `mergerThreshold`, `splitterThreshold`, `parentSubspace`, `childSubspaces`.
*   **Density Calculation:** Implement a function `calculateDensity(subspace)` that returns the number of registered objects within the subspace, normalized by the subspace volume.
*   **Merge Operation:**
    *   `mergeSubspaces(subspaceA, subspaceB)`:  If `calculateDensity(subspaceA) + calculateDensity(subspaceB) > mergerThreshold`, then:
        1.  Transfer all registered objects from `subspaceB` to `subspaceA`.
        2.  Update `subspaceA`'s bounding volume to encompass `subspaceB`'s volume.
        3.  Set `subspaceB`’s `parentSubspace` to `subspaceA`.
        4.  Remove `subspaceB` from the active subspace list.
        5.  Adjust object registration data to reflect the merge.
*   **Split Operation:**
    *   `splitSubspace(subspace)`: If `calculateDensity(subspace) > splitterThreshold`:
        1.  Calculate the longest axis of the subspace's bounding volume.
        2.  Divide the subspace along that axis into two new subspaces (approximately equal volume).
        3.  Distribute objects to the new subspaces based on their spatial location.
        4.  Set `parentSubspace` of the new subspaces to the original `subspace`.
        5.  Add the new subspaces to the active subspace list.
        6.  Adjust object registration data to reflect the split.
*   **Adaptive Trigger:**  Implement a background thread that periodically (or event-driven) checks subspace densities and triggers merge/split operations as needed.  Configurable thresholds per subspace type.
*   **User Interaction Influence:**  When a user interacts with objects in a subspace, temporarily lower the `splitterThreshold` for that subspace to create more granular detail around the interaction point.
*   **Computational Load Balancing:**  Monitor the rendering/processing load of each subspace.  If a subspace is overloaded, trigger a split to distribute the load.

**Pseudocode:**

```
// Background thread loop
while (running) {
  for each subspace in activeSubspaces {
    density = calculateDensity(subspace)
    if (density > subspace.mergerThreshold) {
      findNeighboringSubspaces(subspace) //Implement this
      mergeWithLowestDensityNeighbor(subspace) //Implement this
    }
    if (density > subspace.splitterThreshold) {
      splitSubspace(subspace)
    }
  }
  sleep(interval)
}

function splitSubspace(subspace) {
  longestAxis = findLongestAxis(subspace.volume)
  midpoint = subspace.volume.center + longestAxis * (subspace.volume.size / 2)

  newSubspaceA = createSubspace(midpoint, subspace.volume.size / 2)
  newSubspaceB = createSubspace(midpoint, subspace.volume.size / 2)

  for each object in subspace.registeredObjects {
    if (object.position.x < midpoint.x) {
      newSubspaceA.registerObject(object)
    } else {
      newSubspaceB.registerObject(object)
    }
  }

  subspace.registeredObjects = []
  subspace.addChild(newSubspaceA)
  subspace.addChild(newSubspaceB)
}
```

This allows the virtual space to adapt to dynamic content and user behavior, optimizing performance and creating more detailed experiences where needed.  It moves beyond a static division of space into a living, breathing environment.