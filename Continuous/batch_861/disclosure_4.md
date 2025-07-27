# 8566707

## Dynamic Reflow with Per-Object Style Sheets

**Concept:** Extend the image-based reflowable file concept to incorporate per-object styling rules, creating a richer, more adaptable rendering system beyond simple positioning.  Instead of *just* reflowing based on bounding box size and display dimensions, we introduce a lightweight styling language embedded within the reflow file, allowing for dynamic visual adjustments.

**Specifications:**

**1. Reflow File Structure (Extension of Existing):**

*   **Reflow File Header:** Standard metadata (version, creator, etc.).
*   **Style Sheet Section:**  A structured section containing style rules applied to individual reflow objects. Style rules are expressed in a custom, lightweight language (see section 3).
*   **Object Data Section:** Contains the image data for each reflow object, bounding region coordinates, and a unique object ID that links it to a corresponding style rule in the Style Sheet Section.  This is similar to the current system, but adds the Object ID.

**2. Object ID Scheme:**

*   **Format:** UUID (Universally Unique Identifier).  Ensures uniqueness even across multiple reflow files.
*   **Purpose:**  Acts as the key for linking each reflow object to its specific style rule(s).

**3. Style Sheet Language (Reflow Style Language – RSL):**

*   **Syntax:** Key-value pairs within a block associated with an Object ID.  Uses a simplified CSS-like syntax for ease of parsing.
*   **Supported Properties:** (Extendable)
    *   `scale`:  Floating-point value for scaling the object.
    *   `rotation`: Angle in degrees for rotating the object.
    *   `opacity`:  Floating-point value between 0 and 1 for transparency.
    *   `border`: Specification for a border (color, width, style).
    *   `shadow`: Specification for a shadow effect (color, offset, blur).
    *   `filter`: Applies image filters (blur, sharpen, sepia, etc.).
    *   `text-align`: Alignment for text-based reflow objects (left, center, right).
*   **Example RSL Entry:**

```
{
  "object_id": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
  "scale": 1.2,
  "rotation": 5,
  "border": "1px solid #000000",
  "shadow": "2px 2px 5px #888888"
}
```

**4. Rendering Pipeline Modification:**

*   **Parsing:**  The rendering engine must parse the Style Sheet Section *before* rendering any objects. This creates a style lookup table indexed by Object ID.
*   **Style Application:**  During rendering, *after* determining the horizontal and vertical render positions, the engine retrieves the associated style rules from the lookup table and applies them to the object's image data *before* it’s drawn.
*   **Dynamic Style Overrides:** Allow for runtime style overrides via API calls. This enables interactive customization of the rendered content.

**5. Zoom Level Integration:**

*   **Scaling Factor Calculation:**  Based on the display zoom level, a global scaling factor is determined.
*   **Style Application Adjustment:** Apply both the global scaling factor *and* the individual `scale` property from the RSL to each object.  This allows for combined scaling effects.

**Pseudocode (Rendering Loop Snippet):**

```
For each reflow object in object_data:
  object_id = object_data[i].id
  x = calculate_horizontal_position(object_data[i].bounding_box, display_width)
  y = calculate_vertical_position(object_data[i].bounding_box, display_height)

  style = style_lookup_table[object_id] // Retrieve style from lookup table

  // Apply style properties
  scaled_image = scale_image(object_data[i].image, style.scale * zoom_level)
  rotated_image = rotate_image(scaled_image, style.rotation)
  opacity_image = apply_opacity(rotated_image, style.opacity)

  draw_image(opacity_image, x, y)
```

**Innovation Focus:** This system moves beyond simply *positioning* reflow objects to actively *styling* them, enabling more visually dynamic and adaptable content rendering.  The lightweight style sheet language offers a balance between flexibility and efficiency.