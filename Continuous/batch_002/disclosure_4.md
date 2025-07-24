# 9865200

## Dynamic Subpixel Borrowing & Color Space Expansion

**Core Concept:** Extend the reflectance control system to not just compensate adjacent subpixels for target adjustments, but to *actively borrow* reflectance capacity between color channels to achieve a wider color gamut and improved contrast.

**Specifications:**

*   **Subpixel Architecture:** Standard RGB subpixel arrangement, electrowetting-based reflectance control. Each subpixel includes a dedicated driver circuit.
*   **Reflectance Capacity Mapping:** Each subpixel (R, G, B) has a defined maximum and minimum reflectance level. These levels are not necessarily symmetrical (e.g., R might have a wider dynamic range than B).  The system maps these ranges into a "Reflectance Capacity Space."
*   **Color Gamut Engine:** A software module that analyzes the target color for each pixel. This engine determines if the target color is within the native gamut of the display.
*   **Borrowing Algorithm:** If a target color is *outside* the native gamut:
    1.  Identify the color channel(s) with available reflectance capacity (i.e., not driven to maximum or minimum).
    2.  Reduce reflectance in the available channel(s) to “borrow” capacity.  This borrowing is constrained – a maximum percentage of a channel’s capacity can be allocated.
    3.  Increase reflectance in the limiting channel(s) (the ones preventing the target color from being displayed) to move closer to the target.
    4.  Adjacent subpixel compensation is *still* applied, but secondary to the borrowing algorithm.
*   **Temporal Smoothing:** To mitigate the visual artifacts caused by rapid borrowing/reallocation, a temporal smoothing filter is applied to the reflectance adjustments. This prevents jarring transitions.
*   **Calibration Routine:** A calibration process is necessary to determine the precise reflectance characteristics of each subpixel and map them into the reflectance capacity space. This also accounts for manufacturing variations.
*   **Driver Circuit Modification:** The driver circuits require an additional control input to manage the “borrowing” function and ensure accurate reflectance adjustments.

**Pseudocode (Borrowing Algorithm - per pixel):**

```
function borrow_reflectance(target_R, target_G, target_B) {

    current_R = read_reflectance(R_subpixel);
    current_G = read_reflectance(G_subpixel);
    current_B = read_reflectance(B_subpixel);

    //Determine limiting channel (closest to max reflectance)
    limiting_channel = find_limiting_channel(current_R, current_G, current_B);

    //Calculate required increase in limiting channel
    increase_amount = target_limiting_channel - current_limiting_channel;

    //Identify available channels (reflectance not at min/max)
    available_channels = find_available_channels(current_R, current_G, current_B);

    //Distribute decrease across available channels
    for each channel in available_channels {
        decrease_amount = min(increase_amount / length(available_channels), max_borrow_percentage * channel_range)
        set_reflectance(channel, channel - decrease_amount)
        increase_amount -= decrease_amount
    }

    //Apply increase to limiting channel
    set_reflectance(limiting_channel, limiting_channel + increase_amount)

    //Apply adjacent subpixel compensation
    apply_compensation(R,G,B)

    //Apply temporal smoothing
    smooth_reflectance(R,G,B)

}
```

**Potential Benefits:**

*   Wider color gamut.
*   Improved contrast ratios.
*   More vibrant and realistic images.