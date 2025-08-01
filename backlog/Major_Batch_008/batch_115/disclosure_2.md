# 10127868

## Dynamic Subpixel Masking for Enhanced Perceived Resolution

**Concept:** Expand upon the subpixel control mechanism by introducing dynamically generated, per-frame masks that refine the effective subpixel arrangement. This moves beyond simple subpixel selection for color/brightness and allows for creating the *illusion* of higher spatial resolution.

**Specs:**

*   **Display Core:** Electrowetting display array (as per the provided patent) with individually addressable subpixels (R, G, B assumed, but adaptable to other primaries).
*   **Resolution Enhancement Module (REM):** A dedicated hardware/software module operating in parallel with the display controller.
*   **Input:** High-resolution source image data.
*   **REM Processing:**
    1.  **Image Analysis:** The REM analyzes the source image, identifying edges, textures, and regions of high detail.
    2.  **Mask Generation:** Based on the analysis, the REM generates a per-frame ‘mask’ defining a refined subpixel arrangement. This mask isn't a simple upscaling – it's a re-mapping of the source pixels to the physical subpixels, potentially ‘borrowing’ subpixels from neighboring pixels to sharpen edges or enhance textures. The mapping will *not* be consistent across the entire image.
    3.  **Subpixel Control Data:** The REM outputs control data to the display controller, specifying which subpixels should be activated and to what degree, *based on the generated mask*. This overrides the standard subpixel selection.
*   **Mask Types:**
    1.  **Edge Sharpening Mask:** Subpixels along edges are ‘staggered’ to create a visually sharper transition.
    2.  **Texture Enhancement Mask:** Subpixels are dynamically allocated to emphasize fine textures, creating the illusion of greater detail.
    3.  **Directional Smoothing Mask:**  In areas of gradual color change, subpixels are averaged in a directional manner (e.g., horizontal or vertical) to reduce visible pixelation and create smoother transitions.
*   **Adaptive Masking:**
    *   The REM monitors the performance of the masking algorithm (e.g., visual artifact detection) and dynamically adjusts the masking parameters to optimize the perceived resolution and minimize artifacts.
    *   The intensity of the masking effect is also adaptive.  In regions of low detail, the masking effect is reduced or disabled.
*   **Hardware Acceleration:** The REM requires dedicated hardware acceleration to perform the image analysis, mask generation, and subpixel control data generation in real-time, without impacting the display refresh rate.
*   **Pseudocode (REM Core):**

```pseudocode
function generate_mask(source_image):
  mask = empty_image(same_size_as source_image)

  for each pixel in source_image:
    edge_detected = detect_edge(pixel, surrounding_pixels)
    texture_detected = detect_texture(pixel, surrounding_pixels)

    if edge_detected:
      mask[pixel] = apply_edge_sharpening_pattern()
    else if texture_detected:
      mask[pixel] = apply_texture_enhancement_pattern()
    else:
      mask[pixel] = apply_directional_smoothing_pattern()

  return mask

function apply_mask_to_display(source_image, mask):
  subpixel_control_data = empty_array()

  for each subpixel in display:
    corresponding_mask_value = get_mask_value(subpixel, mask)
    subpixel_control_data.append(corresponding_mask_value)

  send_subpixel_control_data_to_display(subpixel_control_data)

```

**Potential Benefits:**

*   Enhanced perceived resolution without increasing the physical pixel density.
*   Improved image sharpness and detail.
*   Reduced pixelation and artifacts.
*   More immersive visual experience.