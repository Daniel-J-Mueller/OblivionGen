# 9804383

## Microfluidic Electrowetting Lens Array

**Concept:** Utilize the multi-layer support plate technology to create a dynamically adjustable lens array for micro-endoscopy and high-resolution imaging. Instead of simply controlling fluid movement for displays, we’ll shape fluids into micro-lenses, and *actively* control their focal length and curvature using electrowetting.

**Specifications:**

*   **Substrate:** A silicon wafer with pre-fabricated micro-chambers. Each chamber will act as the housing for a single micro-lens.
*   **Multi-Layer Support Plate:** Each micro-chamber receives a support plate built according to the patent, but optimized for lens creation.
    *   **First Layer (Inorganic):** Silicon Nitride (SiNₓ) – provides a robust, electrically insulating base layer with low defect density. Thickness: 50nm.
    *   **Second Layer (Organic):** A conductive polymer blend – PEDOT:PSS doped with graphene quantum dots for enhanced conductivity and mechanical flexibility. Thickness: 75nm. This layer *is* the electrode, and will define the active area of the lens.
*   **Working Fluid:** A high refractive index fluid (e.g., Fluorinert FC-70) containing dispersed metal nanoparticles (e.g. gold or silver) to enhance light manipulation and create sharper focal points.
*   **Chamber Geometry:** Micro-chambers will be designed with a slight curvature. This pre-defined curvature, combined with the electrowetting-driven fluid deformation, will create the final lens shape. Chamber diameter: 50-200μm. Chamber depth: 25-75 μm
*   **Electrode Patterning:** Each chamber’s conductive polymer layer will be patterned with concentric ring electrodes. This allows for fine-grained control of the fluid surface tension and, therefore, the lens shape. Ring electrode width: 5-10μm. Ring spacing: 2-5μm.
*   **Control System:** A custom-designed microcontroller and driver circuitry will apply variable voltages to each ring electrode, creating an electric field that deforms the fluid surface.
*   **Assembly:** The substrate, multi-layer support plates, and fluid are assembled in a cleanroom environment to minimize contamination. A top plate, also with micro-chambers, seals the fluid within each chamber.
*   **Array Configuration:** The micro-lenses will be arranged in a 2D array (e.g. 10x10). Each lens can be individually controlled.

**Pseudocode (Control Algorithm):**

```
//Define lens parameters for each lens in the array
Lens[x,y].target_focal_length = desired_focal_length;
Lens[x,y].target_curvature = calculated_curvature;

//For each lens in the array:
For x = 0 to Array_Width - 1:
    For y = 0 to Array_Height - 1:
        //Calculate voltage for each ring electrode based on target curvature
        For ring = 0 to Num_Rings - 1:
            voltage[ring] = CalculateVoltage(ring, target_curvature, ring_spacing);

        //Apply voltages to electrodes
        ApplyVoltage(voltage);

        //Feedback loop to maintain desired curvature
        ReadCurvatureSensor();
        error = target_curvature - measured_curvature;
        voltage_adjustment = proportional_gain * error;
        voltage = voltage + voltage_adjustment;
    End For
End For
```

**Potential Applications:**

*   Minimally invasive endoscopy
*   High-resolution microscopy
*   Adaptive optics for imaging through turbid media
*   Compact imaging systems for mobile devices
*   Dynamic beam steering and focusing