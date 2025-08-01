# 9529189

**Electrowetting Display with Dynamic Refractive Index Matching Layers**

**Concept:** Enhance luminance and color accuracy by incorporating dynamically adjustable refractive index matching layers *around* the electrowetting elements, rather than solely relying on reflective spacers/walls. This approach aims to minimize light loss due to refraction at the interfaces between different materials within the display stackup.

**Specs:**

*   **Layer Stack:** Electrowetting display with standard layers (substrates, partition walls, electrowetting elements – oil, conductive fluid, hydrophobic layer). *Addition*: Two thin-film layers surrounding each electrowetting element: an inner layer directly adjacent to the conductive fluid, and an outer layer adjacent to the substrate.
*   **Material – Inner Layer:** A microfluidic channel filled with a fluid containing nanoparticles with controllable refractive index. Index control achieved via electrical field application (electrophoresis of charged nanoparticles) or thermal control (temperature-sensitive polymers swelling/contracting). The refractive index range of this layer should be tunable between 1.3 - 1.6.
*   **Material – Outer Layer:** A transparent polymer film containing micro-lenses or diffractive optical elements (DOEs). These elements are *also* dynamically adjustable via shape memory polymers (SMPs) or micro-actuators. The SMPs/micro-actuators are controlled by the same system controlling the inner layer’s refractive index.
*   **Control System:** A dedicated micro-controller and driver circuitry. This system receives pixel-level color and luminance data, calculates optimal refractive index and micro-lens/DOE configurations for each pixel, and applies the necessary voltages/thermal inputs to control the microfluidic channels, SMPs, and micro-actuators.
*   **Pixel Architecture:** Each pixel comprises an array of sub-pixels, each with its own independently controlled refractive index matching layers. This allows for fine-grained control over light propagation within each sub-pixel.
*   **Refractive Index Mapping Algorithm:** A predictive algorithm which determines the optimal refractive index and micro-lens/DOE configuration for each sub-pixel based on the desired color, luminance, and viewing angle. The algorithm should account for material properties, display stackup, and ambient lighting conditions.
*   **Sensor Integration:** Ambient light sensors integrated into the display to provide real-time feedback to the refractive index mapping algorithm, enabling adaptive optimization.

**Pseudocode - Refractive Index Control Loop (per subpixel):**

```
// Initialize: Read target color (R,G,B) and luminance (L)
// Read ambient light intensity (I)
// Calculate ideal refractive index (n_ideal) based on R,G,B, L, and I

// Loop:
// 1. Measure current refractive index (n_current) using optical sensors
// 2. Calculate error (error = n_ideal - n_current)
// 3. Apply voltage/thermal input to microfluidic channel
// 4. Monitor n_current until within tolerance (e.g., 0.001)
// 5. Adjust SMP/micro-actuator configuration to fine-tune light propagation
// 6. Repeat steps 3-5 until optimal light output achieved
// 7. Next subpixel
```

**Potential benefits:**

*   Significantly improved luminance and color accuracy
*   Wider viewing angles
*   Reduced power consumption (by minimizing light loss)
*   Dynamic adaptation to changing ambient lighting conditions.
*   Potential for polarization control for improved contrast.