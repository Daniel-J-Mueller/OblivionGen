# 9728561

**Stacked TFT with Integrated Micro-Lens Array for Enhanced Light Management**

**Concept:** Integrate a micro-lens array directly into the dielectric layer above the pixel electrode of the stacked TFT structure. This would allow for precise light control, directing illumination either *towards* or *away* from the display surface for improved contrast, viewing angle, and potentially, power efficiency.

**Specs:**

*   **Substrate:** Standard display substrate (glass, flexible polymer, etc.).
*   **TFT Stack:** Utilize the core fabrication steps outlined in the provided patent – dual TFT structure, insulating layers, common electrode, dielectric material.
*   **Dielectric Material Modification:** The dielectric material (silicon nitride or similar) will be engineered with micro-lens shaped cavities etched into its surface. These cavities will be precisely molded/etched during deposition.  Cavity dimensions (diameter, depth, spacing) will be customizable based on display resolution and desired viewing characteristics.  Target cavity dimensions: 5-50 microns diameter, 1-10 micron depth, 10-100 micron pitch.
*   **Lens Material:** Deposit a high refractive index transparent material (e.g., TiO2, ZnS, or a specifically formulated polymer) *inside* the etched cavities to form the micro-lenses.  Material selection based on desired refractive index and compatibility with subsequent processing steps.
*   **Pixel Electrode Integration:** Pixel electrode deposition process needs to ensure conformal coverage over the micro-lens array.
*   **Polarizer Alignment:**  Display design must account for polarizer alignment relative to the micro-lens array to maximize contrast and minimize glare.

**Pseudocode for Micro-Lens Array Fabrication (integrated into dielectric deposition):**

```
FUNCTION DepositDielectricWithLenses(substrate, lens_specifications):
    // lens_specifications:  dictionary containing lens diameter, depth, pitch, material
    
    // 1. Deposit initial dielectric layer
    DEPOSIT DielectricMaterial(substrate, thickness = base_dielectric_thickness)
    
    // 2. Pattern micro-lens cavities
    USING Lithography and Etching:
        CREATE Mask based on lens_specifications (periodic array of circular openings)
        EXPOSE Mask onto dielectric layer
        ETCH dielectric layer to create cavities matching mask dimensions.
    
    // 3. Deposit lens material into cavities
    DEPOSIT LensMaterial(substrate, thickness = cavity_depth) // conformal deposition
    
    // 4. Planarize (optional)
    // Apply planarization process to ensure a flat surface over the lenses
    
    RETURN substrate // with dielectric layer and integrated micro-lens array
```

**Potential Enhancements:**

*   **Active Control:** Explore the possibility of using electro-active polymers or liquid crystals *within* the lens structure to dynamically adjust the focal length and direction of light.  This could enable variable viewing angles or create 3D effects.
*   **Diffractive Optics:** Integrate diffractive elements *on* the surface of the lenses to further manipulate light and enhance image quality.
*   **Quantum Dot Integration:**  Incorporate quantum dots into the lens material to create highly efficient and color-pure light emitters.
*   **Multi-Layer Lenses:** Stack multiple lens layers with varying refractive indices to create more complex optical systems.