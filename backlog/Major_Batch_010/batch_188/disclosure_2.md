# 9588336

## Microfluidic Lens Array for Dynamic Focus Electrowetting Displays

**Concept:** Integrate a microfluidic lens array *within* the liquid duct system of the electrowetting display to create a dynamically focusing display. Instead of solely controlling liquid movement for color/image, manipulate the *shape* of microfluidic lenses to control focal depth and create 3D effects or adjustable sharpness.

**Specs:**

*   **Microfluidic Lens Material:** Transparent PDMS (Polydimethylsiloxane) – chosen for its flexibility, optical clarity, and ease of microfabrication.
*   **Lens Array Geometry:** Hexagonal close-packing arrangement of spherical micro-lenses. Hexagonal packing maximizes lens density and minimizes wasted space.  Lens diameter: 50-200um.
*   **Lens Actuation:** Each micro-lens is connected to a dedicated micro-channel fed by a localized electrowetting pump (integrated into the existing liquid duct system described in the patent). By varying the voltage applied to the electrowetting pump, the volume of fluid within the micro-channel is adjusted, deforming the micro-lens and changing its focal length.
*   **Layer Stack:**
    1.  Support Plate (existing from patent) – with electrode layer.
    2.  Sacrificial Layer – etched to form liquid ducts *and* micro-lens cavities.
    3.  PDMS Micro-Lens Array – fabricated *in situ* within the etched cavities.  PDMS bonded to sacrificial layer.
    4.  Spacer Grid – integrated with PDMS array to maintain consistent separation.
    5.  Top Support Plate.
*   **Electrode Integration:** Each micro-lens is positioned directly above a corresponding micro-electrode on the support plate. This allows precise control of the fluid movement within the micro-channel via electrowetting.
*   **Fluid:**  Dielectric fluid compatible with electrowetting. Index of refraction optimized for lens performance.
*   **Control System:**  A dedicated microcontroller controlling voltage applied to each micro-electrode, allowing for dynamic control of lens shape and focus.  Image processing algorithms to map 3D scene data to individual lens control signals.
*   **Fabrication Process (Adaptation of Patent Process):**
    1.  Deposit sacrificial layer on support plate.
    2.  Etch sacrificial layer to create not only liquid ducts, but also cavities for micro-lenses.  Use a two-mask process, one for ducts, one for lens cavities.
    3.  Deposit PDMS precursor into lens cavities and cure.
    4.  Deposit photoresist spacer grid.
    5.  Remove remaining sacrificial layer.
    6.  Bond top support plate.

**Pseudocode (Lens Control):**

```
// Input: 3D Scene Data (depth map)
// Output: Voltage Control Signals for each Micro-Lens

function adjust_lens_focus(depth_map, lens_array):
    for each lens in lens_array:
        // Calculate required focal length based on depth at lens position
        required_focal_length = calculate_focal_length(depth_map[lens.position])

        // Calculate voltage needed to achieve required focal length
        voltage = calculate_voltage(required_focal_length)

        // Set voltage on corresponding micro-electrode
        set_electrode_voltage(lens.electrode, voltage)
    end for
end function
```

**Potential Applications:**

*   Augmented Reality displays
*   Holographic displays
*   Variable-focus microscopes
*   Improved image sharpness and depth of field in mobile devices.