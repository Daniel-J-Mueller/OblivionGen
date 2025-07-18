# 10209508

## Micro-Lens Array Integration for Enhanced Color Mixing & Viewing Angle

**Concept:** Integrate a micro-lens array (MLA) directly onto the second support plate, *above* the color filter layer, but *underneath* the recessed region/ITO layer. This MLA will be tailored to focus and redirect light emitted/reflected from each sub-pixel, improving color mixing and widening the effective viewing angle of the display.

**Specifications:**

*   **MLA Material:** Polymeric material with high transparency ( >95% transmission across visible spectrum) and refractive index matching the surrounding layers.  Consider cyclo-olefin polymer (COP) or polymethylmethacrylate (PMMA).
*   **Lens Shape:**  Aspheric lenses. This is to minimize aberrations and maximize focusing precision. Lenslet diameter: 20-50 micrometers.
*   **Lens Pitch:** Matched to subpixel size. 1:1 mapping for optimal light control.
*   **MLA Fabrication:**  Nanoimprint lithography or UV embossing for high-resolution, large-area fabrication.  
*   **Alignment:** Precise alignment of MLA with color filters and subpixels (tolerance < 1 micrometer) is critical. This may require initial registration marks during fabrication followed by self-alignment during layer stacking.
*   **Recessed Region Adaptation:** The recessed region containing the ITO layer will be *slightly* deepened to accommodate the height of the MLA *without* increasing the overall display thickness.  The spacer between the pixel wall and ITO will need to be adjusted to account for the new recessed depth.
*   **Surface Treatment:** Anti-reflective coating applied to the MLA surface to further improve light transmission.

**Operational Details:**

1.  Light generated or reflected from each subpixel passes *through* the corresponding MLA lenslet.
2.  The lenslet focuses and redirects the light, slightly diverging it. This widening of the beam improves off-axis viewing.
3.  The diverging light from multiple subpixels mixes more effectively, creating a more uniform and vibrant color appearance.
4.  The MLA can be designed with different lenslet shapes and arrangements to optimize color mixing and viewing angle for specific applications. For example, an elliptical lenslet shape could focus light in a specific direction to improve contrast.

**Pseudocode for MLA Design Optimization:**

```
FUNCTION optimizeMLA(subpixelSize, colorFilterArrangement, targetViewingAngle)
    // INPUT: Subpixel size, color filter arrangement, desired viewing angle
    // OUTPUT: Optimized MLA parameters (lenslet shape, pitch, refractive index)

    // Initialize optimization parameters
    lensletShape = "spherical"
    lensletPitch = subpixelSize
    refractiveIndex = 1.5

    // Define objective function: maximize color uniformity and viewing angle
    OBJECTIVE = colorUniformity(lensletShape, lensletPitch, refractiveIndex) + viewingAngle(lensletShape, lensletPitch, refractiveIndex)

    // Use an optimization algorithm (e.g., genetic algorithm, simulated annealing) to find the optimal MLA parameters
    WHILE not converged
        // Generate a random set of MLA parameters
        NEW_MLA_PARAMETERS = generateRandomParameters()

        // Evaluate the objective function for the new parameters
        NEW_OBJECTIVE = colorUniformity(NEW_MLA_PARAMETERS) + viewingAngle(NEW_MLA_PARAMETERS)

        // If the new parameters improve the objective function, accept them
        IF NEW_OBJECTIVE > OBJECTIVE
            OBJECTIVE = NEW_OBJECTIVE
            lensletShape = NEW_MLA_PARAMETERS.lensletShape
            lensletPitch = NEW_MLA_PARAMETERS.lensletPitch
            refractiveIndex = NEW_MLA_PARAMETERS.refractiveIndex
        ENDIF
    ENDWHILE

    RETURN lensletShape, lensletPitch, refractiveIndex
END
```

This approach enables a significantly improved display experience with wider viewing angles and enhanced color mixing, building upon the existing electrowetting technology.