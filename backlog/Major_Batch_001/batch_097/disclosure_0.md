# 10074321

## Dynamic Metameric Mapping with Perceptual Weighting

**Concept:** Expand upon the metameric transfer value idea in the patent to create a dynamic, perceptually-weighted mapping system. Instead of simply distributing a transfer value based on neighboring pixel brightness, the system analyzes the *perceived* color difference between the target color of a subpixel and its achievable reflectance, then distributes a weighted value to maximize visual uniformity.

**Specs:**

1.  **Perceptual Color Model Integration:** Incorporate a perceptually uniform color space (e.g., CIELAB, CIECAM02) into the reflectance quantization error calculation. This will move beyond simple RGB/grayscale differences and more closely align with human vision.

2.  **Error Metric:** Define a perceptual error metric (ΔE) to quantify the difference between the target color (based on the source image) and the achievable reflectance of a subpixel given the electrowetting display's limitations.  ΔE = f(L*, a*, b* target, L*, a*, b* achievable) where L*, a*, b* represent the CIELAB color coordinates.

3.  **Dynamic Weighting Function:** Develop a weighting function that assigns different weights to neighboring subpixels based on their proximity and perceptual contribution.  For example:
    *   `Weight(pixel_i, pixel_j) = exp(-Distance(pixel_i, pixel_j)^2 / (2 * σ^2))`
    *   Where σ is a tuning parameter controlling the influence radius. Closer pixels have higher weights.

4.  **Metameric Transfer Value Calculation:**
    *   For a "white" subpixel (or any subpixel needing correction):
        *   Calculate the ΔE between the target and achievable reflectance.
        *   Determine a "metameric transfer value" (MTV) based on ΔE, scaled by a maximum allowable transfer value.  MTV = k * ΔE, where k is a proportionality constant.

5.  **Weighted Distribution Algorithm:**
    *   Identify neighboring subpixels within a defined radius.
    *   Calculate the weight for each neighbor using the weighting function.
    *   Distribute the MTV proportionally to the weights of the neighbors.
    *   `Reflectance(neighbor_i) = Reflectance(neighbor_i) + (MTV * Weight(neighbor_i) / Σ Weight(all neighbors))`

6.  **Adaptive Sigma:** Implement an algorithm to dynamically adjust the σ parameter (influence radius) based on image content. High-detail regions may benefit from a smaller σ, while smoother areas can use a larger σ. This could be implemented via an edge detection algorithm.

7.  **Hardware Integration:** The system requires a lookup table (LUT) to map target colors to achievable reflectances, considering the electrowetting display’s limitations. This LUT must be pre-calculated and stored in the display controller.  The display controller should implement the weighting and distribution algorithms in real-time.

**Pseudocode:**

```
// For each subpixel 'white_pixel' needing correction:
Calculate DeltaE = PerceptualColorDifference(target_color, achievable_reflectance);
metameric_transfer_value = k * DeltaE;
neighbors = FindNeighboringSubpixels(white_pixel, radius);
total_weight = 0;

For each neighbor in neighbors:
  weight = CalculateWeight(white_pixel, neighbor);
  total_weight += weight;

For each neighbor in neighbors:
  weight_fraction = weight / total_weight;
  neighbor.reflectance += (metameric_transfer_value * weight_fraction);
```