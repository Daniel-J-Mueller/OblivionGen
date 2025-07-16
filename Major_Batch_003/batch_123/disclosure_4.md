# 11909993

## Dynamic Partitioning with Adaptive Precision Interpolation

**Concept:** Extend the parallel pipeline approach to not just refine existing motion vectors, but to *dynamically partition* coding units based on motion complexity *during* the refinement process, and then apply varying levels of interpolation precision based on that partitioning.

**Rationale:** The current patent focuses on parallelizing refinement of *pre-defined* coding units.  Motion isn’t uniform across a frame. Areas of high motion complexity benefit from smaller coding units for accurate representation, while simpler areas can use larger units for efficiency.  This system addresses that by dynamically adjusting unit size during the refinement process, and adapting interpolation precision to match.

**System Specs:**

*   **Motion Complexity Analyzer (MCA):**  A pre-pipeline module that analyzes integer-level motion vectors to estimate local motion complexity within a coding unit.  Output is a 'complexity score' (0-100).  This could be a variance calculation of the motion vectors or a gradient analysis.
*   **Dynamic Partitioning Engine (DPE):**  Resides *within* the parallel pipeline. Receives the complexity score from the MCA. Based on a pre-defined threshold (adjustable), it dynamically splits the current coding unit into smaller units *before* sub-pixel refinement.  
    *   Thresholds are configurable. For example:
        *   Complexity Score < 20: No split
        *   20 <= Complexity Score < 50: Split into 4 equal units
        *   Complexity Score >= 50: Split into 16 equal units
*   **Adaptive Interpolation Module (AIM):** Operates *after* DPE, within the pipeline.  Receives the split coding unit data and applies sub-pixel refinement with dynamically adjusted precision.
    *   **Precision Levels:**
        *   **Level 1 (Low):** Half-pixel interpolation only. For low-complexity units.
        *   **Level 2 (Medium):** Half and Quarter-pixel interpolation.  For medium-complexity units.
        *   **Level 3 (High):** Half, Quarter, and One-Eighth-pixel interpolation. For high-complexity units.
*   **Parallel Pipeline Architecture:** Maintains the row-based parallel processing from the original patent. Each row pipeline now incorporates the MCA, DPE, and AIM modules.

**Pseudocode (within a single Row Pipeline):**

```
FOR each CodingUnit in Row:
    ComplexityScore = MCA(CodingUnit.IntegerMotionVectors)

    IF ComplexityScore > SplitThreshold:
        SplitCodingUnit = DPE(CodingUnit, ComplexityScore) // Generates smaller CodingUnits
        FOR each SubUnit in SplitCodingUnit:
            InterpolationLevel = DetermineInterpolationLevel(SubUnit.ComplexityScore)
            RefinedMotionVectors = AIM(SubUnit, InterpolationLevel)
            //Store RefinedMotionVectors
    ELSE:
        InterpolationLevel = DetermineInterpolationLevel(CodingUnit.ComplexityScore)
        RefinedMotionVectors = AIM(CodingUnit, InterpolationLevel)
        //Store RefinedMotionVectors
END FOR
```

**Hardware Implications:**

*   Requires addition of MCA and DPE modules within each parallel pipeline.
*   AIM requires configurable interpolation hardware to support varying precision levels.
*   Increased memory bandwidth to handle potentially larger number of smaller coding units.

**Potential Benefits:**

*   Improved compression efficiency by adapting coding unit size to motion complexity.
*   Reduced artifacts in high-motion areas.
*   More efficient use of computational resources by prioritizing precision based on need.