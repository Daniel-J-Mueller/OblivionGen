# 9728561

**Stacked TFT with Integrated Micro-Lens Array for Enhanced Light Management**

**Concept:** Extend the stacked TFT architecture to incorporate a micro-lens array directly *within* the dielectric layers, above the pixel electrode. This enables precise light control for improved display characteristics – contrast, brightness, viewing angle. The lenses are formed *during* the dielectric deposition process, eliminating a separate manufacturing step.

**Specifications:**

1.  **TFT Stack:** Standard stacked TFT structure as described in the reference patent (two TFTs, insulating layers, common electrode).

2.  **Dielectric Layer – Lens Formation:**
    *   Material: Use a high refractive index dielectric material (e.g., SiN, TiO2) for both the primary dielectric layer *and* the micro-lens formation.
    *   Deposition: Utilize a patterned deposition technique (e.g., shadow masking, localized plasma etching during deposition, or a template-assisted process).
    *   Patterning: The pattern consists of closely spaced, convex micro-lenses *above* the pixel electrode. Lens diameter: 5-20 μm. Lens pitch: 10-30 μm. Lens height/profile: Controlled by deposition parameters or etching depth.  The lenses are fabricated *concurrently* with the dielectric layer deposition. No separate lens deposition step. The lensing will be in the plane of the dielectric layer.
    *   Multi-Layer Lensing: Implement multiple dielectric layers with differing refractive indices to create more complex lensing profiles (e.g., aspheric lenses).

3.  **Pixel Electrode Modification:**
    *   Surface Texture: Introduce a slight texture or scattering element on the pixel electrode surface to enhance light diffusion and minimize glare.
    *   Reflectivity Control: Optimize the pixel electrode reflectivity to minimize light loss.

4.  **Control Scheme:**
    *   Individual Lens Control: If feasible, incorporate micro-heaters or electro-optic materials *within* the lens structure to enable dynamic adjustment of the lensing effect. This would allow for active viewing angle control or contrast enhancement on a pixel-by-pixel basis.
    *   Polarization Control: Integrate a polarization layer between the lenses and the display surface to further refine light management and reduce reflections.

**Pseudocode for Manufacturing Process (Lens Formation):**

```
// Process: Concurrent Dielectric/Lens Deposition
// Inputs: Substrate with Stacked TFT Structure, High Refractive Index Dielectric Material
// Outputs: Substrate with Stacked TFT + Integrated Micro-Lens Array

1.  Prepare Substrate: Clean and pre-treat the substrate with the completed TFT stack.
2.  Mask Creation: Create a shadow mask with micro-lens patterns. (Or template for template assisted creation)
3.  Dielectric Deposition: Deposit the dielectric material using a technique that creates the micro-lens shape through the mask.
    *   Technique Options:
        *   Shadow Masking + Sputtering/Evaporation
        *   Localized Plasma Etching During Deposition
        *   Template-Assisted Growth
    *   Control Parameters: Deposition rate, gas flow, plasma power, temperature.
4.  Layering (Optional): Repeat steps 3 & 4 with different dielectric materials to create multi-layer lenses.
5.  Post-Processing: Remove the mask (if used) and perform any necessary cleaning or annealing.
6.  Pixel Electrode Creation: Continue with the pixel electrode deposition as per the original patent.
```

**Potential Benefits:**

*   Enhanced Brightness and Contrast
*   Wider Viewing Angles
*   Reduced Glare and Reflections
*   Improved Power Efficiency
*   Compact Display Design (integrated lenses eliminate the need for separate lens sheets)
*   Potential for Dynamic Viewing Angle Control