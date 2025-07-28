# 9140894

## Adaptive Pixel Geometry with Micro-Lens Array

**Concept:** Enhance contrast and viewing angle in electro-wetting displays by dynamically adjusting pixel shape using micro-actuators coupled with a micro-lens array.

**Specs:**

*   **Pixel Structure:**  Instead of fixed rectangular pixels, each pixel consists of multiple micro-cells (e.g., 4x4 grid within the standard pixel area).  These micro-cells are formed by micro-actuators – small, transparent polymer structures capable of slight expansion or contraction.
*   **Actuation Mechanism:** Each micro-cell incorporates a micro-chamber filled with an electro-responsive fluid (similar to the core fluid in the electro-wetting display). Applying a voltage to a control electrode embedded *within* the actuator causes the fluid to change volume, subtly deforming the shape of the micro-cell.
*   **Micro-Lens Array:** A secondary transparent substrate is positioned above the pixel layer, containing a precisely aligned micro-lens array. Each micro-lens corresponds to a cluster of micro-cells.
*   **Control System:** A dedicated control circuit manages voltage applied to both the pixel electrodes *and* the micro-actuator electrodes.  This allows for real-time adjustment of pixel shape and light focusing.
*   **Addressing Scheme:** Each pixel is individually addressable via a matrix addressing scheme. The control circuit dynamically calculates the optimal deformation pattern for each pixel based on viewing angle and desired image content.

**Pseudocode for Control Algorithm (simplified):**

```
// Input: Viewing Angle (theta, phi), Desired Pixel Color (R, G, B)
// Output: Voltage values for Pixel Electrode & Micro-Actuator Electrodes

function calculate_pixel_control(theta, phi, R, G, B):

    // 1. Calculate optimal micro-cell deformation pattern based on viewing angle.
    deformation_pattern = calculate_deformation(theta, phi)

    // 2. Calculate voltage for micro-actuators to achieve the deformation pattern.
    micro_actuator_voltages = calculate_actuator_voltage(deformation_pattern)

    // 3. Calculate voltage for pixel electrode to display desired color.
    pixel_electrode_voltage = calculate_pixel_voltage(R, G, B)

    // 4. Return the calculated voltage values.
    return pixel_electrode_voltage, micro_actuator_voltages
```

**Materials:**

*   Transparent polymer for micro-actuators (e.g., PDMS, Parylene).
*   Electro-responsive fluid within micro-actuators (dielectric fluid).
*   Transparent conductive material for electrodes (ITO, conductive polymers).
*   Optical grade substrate for micro-lens array (e.g., Acrylic, Polycarbonate).

**Expected Benefits:**

*   Increased contrast ratio, especially at off-axis viewing angles.
*   Wider viewing cone.
*   Potential for dynamic brightness adjustment.
*   Novel visual effects through programmed pixel shape changes.