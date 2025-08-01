# 9153157

## Adaptive Reflective/Transmissive Layering for Dynamic Display Segmentation

**Concept:** Expanding upon the idea of segmented monochrome/color areas, this design proposes a physically dynamic layering system *within* each picture element, rather than solely across larger areas.  This allows for granular control over reflectivity and transmissivity *per pixel*, enabling entirely new display modes beyond simple monochrome/color separation. The core innovation is creating a micro-shutter/film stack *within* each pixel, controlled by the single active matrix.

**Specifications:**

*   **Pixel Structure:** Each physical pixel comprises multiple sub-pixel layers. Minimum three layers: 1) Reflective Layer, 2) Transmissive Layer, 3) Dynamic Control Film.
*   **Reflective Layer:** High-reflectivity material (e.g., silver, aluminum) optimized for grayscale reflection.
*   **Transmissive Layer:**  A micro-LED or OLED layer capable of emitting Red, Green, and Blue light.  Density and size optimized for maximum brightness and color fidelity.
*   **Dynamic Control Film:**  A MEMS-based electrically controllable film positioned *between* the reflective and transmissive layers. This film's transparency is adjustable via voltage applied from the active matrix.  Material:  Electro-optic polymer or liquid crystal mixture. This is the core control element.
*   **Active Matrix Control:** The single active matrix addresses each sub-pixel (each layer within a pixel).  Each pixel’s control signal specifies:
    *   Reflective Layer Activation (Brightness level - grayscale).
    *   Transmissive Layer Color & Brightness.
    *   Dynamic Control Film Transparency (0-100%).
*   **Display Modes:**
    *   **Standard Monochrome/Color:** Implement the original patent's functionality.
    *   **Dynamic Grayscale Reflection:** Adjust reflective layer brightness & dynamic film transparency to create grayscale images.
    *   **Transmissive Overlay:**  Display color information *over* a reflective grayscale base, creating a high-contrast effect. (e.g., Reflective grayscale text with color highlights)
    *   **Virtual Lenticular/Barrier Autostereoscopy:** By precisely controlling the transparency of the dynamic control film, create localized light directionality for simple autostereoscopic 3D effects *without* the need for physical lenses or barriers.
    *   **High Dynamic Range (HDR) Enhancement:** Use the reflective layer as a ‘bloom’ effect, enhancing brightness in high-contrast scenes.
*   **Layer Stack Arrangement:** Substrate -> Active Matrix -> Reflective Layer -> Dynamic Control Film -> Transmissive Layer -> Protective Overcoat.
*   **Addressing Scheme:**  Time-division multiplexing (TDM) within the active matrix to control the different layers of each pixel. This requires fast switching speeds for the dynamic control film.
*   **Materials Research:** Focus on electro-optic polymers or liquid crystal mixtures with:
    *   High transparency in the 'off' state.
    *   Fast switching times (sub-millisecond).
    *   High contrast ratio (on/off state).
    *   Wide viewing angle.

**Pseudocode (Active Matrix Control):**

```
for each pixel in display:
  // Reflective Layer Control
  set_reflective_brightness(pixel, grayscale_value)

  // Transmissive Layer Control
  set_transmissive_color(pixel, red_value, green_value, blue_value)
  set_transmissive_brightness(pixel, brightness_value)

  // Dynamic Control Film Control
  set_film_transparency(pixel, transparency_value) // 0-100%

  // Synchronization
  wait_for_frame_end()
```

This system moves beyond static area segmentation to create a truly dynamic display capable of novel visual effects and increased functionality.  The key lies in the ability to control light at the *pixel level* by combining reflection and transmission.