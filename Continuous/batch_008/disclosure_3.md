# 9229221

## Electrowetting Display with Dynamic Fluidic Lenses

**Concept:** Integrate microfluidic channels *within* the second support plate, creating dynamically adjustable lenses formed by controlled displacement of the second fluid. This allows for not just display *of* information, but dynamic optical manipulation, potentially creating a display that can also *focus* or *project* images.

**Specifications:**

*   **Support Plate Material:** Transparent polymer (e.g., PDMS, PMMA) with integrated microfluidic channels etched or molded into the surface facing the second fluid. Channels should be on the nanometer to micrometer scale.
*   **Channel Design:** A network of interconnected microfluidic channels forming an array of individually controllable lens structures.  Channel geometries can vary: spherical, aspherical, cylindrical for diverse optical properties.
*   **Fluid Properties:** Second fluid to be a high-refractive-index fluid compatible with electrowetting. Viscosity needs to be low enough for rapid displacement under electrical control, but high enough to maintain lens shape.
*   **Electrode Configuration:** The second electrode (integrated into the protrusion as per the base patent) will now serve a dual purpose:  electrowetting *and* fluidic channel control. By applying a voltage gradient across the channels, the second fluid can be directed, forming and reshaping the lenses. Electrode material must be biocompatible with the fluids.
*   **Control System:** An array of micro-heaters or piezoelectric actuators integrated with the microfluidic channels, allowing for precise temperature/pressure control.  These would modulate the fluid’s refractive index or physically deform the lens.  A dedicated driver circuit for each lens element.
*   **Addressing Scheme:** A multiplexed addressing scheme, similar to existing LCD or OLED displays, but optimized for the microfluidic lens array.  This involves controlling voltage and current to individual electrodes/actuators.
*   **First Fluid:** Inert, low refractive index fluid. Primary function is to create contrast and separation.
*   **Protrusion Design:** Microfabricated protrusions must house the combined electrode/microchannel structures, ensuring electrical isolation and mechanical stability.
*   **Layer Stack:**
    1.  First Support Plate (transmissive)
    2.  First Fluid
    3.  Second Support Plate (with integrated microfluidic channels and second electrodes)
    4.  Transparent encapsulation layer (to seal the system)

**Pseudocode (Lens Control):**

```
FUNCTION adjust_lens(lens_id, focal_length)
    // lens_id:  Unique identifier for the lens element
    // focal_length: Desired focal length (in mm)

    target_voltage = calculate_voltage(focal_length) // Lookup table/function
    apply_voltage(lens_id, target_voltage) // Send signal to corresponding electrode

    // OPTIONAL:
    monitor_response(lens_id) // Measure actual focal length for calibration
    adjust_voltage(lens_id, error_signal) // Fine-tune voltage for precision
END FUNCTION
```

**Potential Applications:**

*   **Adaptive Displays:** Automatically adjust focus and brightness based on viewing distance and ambient lighting.
*   **Holographic Displays:** Create 3D images by dynamically manipulating light.
*   **Microscopes:**  Automated focusing and image enhancement.
*   **Endoscopes:**  Miniaturized optical systems for medical imaging.
*   **Augmented Reality:**  Dynamic light field generation for realistic AR experiences.