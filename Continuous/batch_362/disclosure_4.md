# 10120184

**Electrowetting Display with Dynamic Refractive Index Partition Walls**

**Concept:** Enhance luminance and contrast by dynamically altering the refractive index of the partition walls themselves, moving beyond simply *being* reflective. This utilizes microfluidic channels *within* the partition walls, filled with fluids of varying refractive indices.

**Specs:**

*   **Partition Wall Material:** Transparent polymer (e.g., PDMS, PMMA) with integrated microfluidic channels. Channel dimensions: 50nm width, 100nm height, spaced 200nm apart within the wall structure.
*   **Microfluidic Fluid 1:** High refractive index fluid (n = 1.7 – 1.9) – e.g., a dense salt solution or a high-refractive-index monomer.
*   **Microfluidic Fluid 2:** Low refractive index fluid (n = 1.3 – 1.4) – e.g., Fluorinert or air.
*   **Actuation:** Integrated micro-heaters or piezoelectric pumps adjacent to each channel. Resolution: Individual control of each channel. Response Time: < 1ms.
*   **Control System:** Algorithm that analyzes the incident light angle and the current electrowetting state of the pixel, then adjusts the refractive index of the partition wall channels to maximize light return *into* the pixel. 
*   **Electrode Integration:** Transparent electrodes embedded within the support plates, patterned to allow for localized control of the electrowetting and microfluidic systems.
*   **Spacer Integration:** Spacers composed of the same transparent polymer as the partition walls, but without the microfluidic channels. 
*   **Layer Stack (Bottom to Top):** Bottom Support Plate (transparent electrode layer), Partition Walls (with microfluidic channels), Electrowetting Elements (hydrophobic layer, oil, transparent fluid), Top Support Plate (transparent electrode layer).
*   **Pixel Density:** Target: 300 PPI (pixels per inch) or greater.
* **Refractive Index Modulation:** Capability to modulate the average refractive index of the partition wall between 1.4 and 1.8.

**Pseudocode (Control Algorithm):**

```
function adjustPartitionWall(incidentAngle, pixelState, partitionWallRefractiveIndex) {
  // Calculate optimal refractive index based on incident angle and pixel state
  optimalRefractiveIndex = calculateOptimalRefractiveIndex(incidentAngle, pixelState)

  // Determine refractive index difference
  refractiveIndexDifference = optimalRefractiveIndex - partitionWallRefractiveIndex

  // Adjust microfluidic channel filling ratios to achieve desired refractive index
  if (refractiveIndexDifference > 0) {
    // Increase high-index fluid proportion
    adjustMicrofluidicPump(channelID, pumpDirection = FILL_HIGH_INDEX, pumpRate = refractiveIndexDifference * 0.1)
  } else {
    // Increase low-index fluid proportion
    adjustMicrofluidicPump(channelID, pumpDirection = FILL_LOW_INDEX, pumpRate = abs(refractiveIndexDifference) * 0.1)
  }

  // Return new refractive index value
  return optimalRefractiveIndex
}
```

**Innovation Rationale:**

Traditional reflective partition walls provide a static boost to luminance. By dynamically controlling the refractive index, we can *steer* light within the pixel, increasing the probability of light being reflected back towards the viewer. This leads to higher contrast, improved viewing angles, and potentially lower power consumption. The microfluidic system allows for precise control, enabling adaptive optimization based on viewing conditions and display content.