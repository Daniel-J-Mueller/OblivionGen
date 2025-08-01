# D972335

## Adaptive Dispenser Morphology

**Concept:** A dispenser device capable of dynamically altering its dispensing aperture *shape* based on the viscosity and volume of the dispensed material, and/or user input. Current dispensers offer volume control, but not aperture *morphology*.

**Specs:**

*   **Core Component:**  A microfluidic "lens" constructed from an array of independently actuatable micro-pillars. These pillars, fabricated from a shape-memory alloy (SMA) or piezoelectric material, can extend and retract, altering the shape of the dispensing orifice.
*   **Sensing:**
    *   **Viscosity Sensor:** Integrated micro-rheometer measuring fluid resistance as it approaches the dispensing point. Data feeds into control system.
    *   **Volume/Flow Sensor:**  Miniature flow meter monitors dispensed volume.
    *   **User Input:**  Touch or voice control allows for pre-set or custom aperture profiles.
*   **Control System:**  A microcontroller (e.g., ESP32) running a closed-loop control algorithm.
    *   Algorithm maps viscosity/volume data to optimal micro-pillar actuation patterns.
    *   User-defined profiles override automated control.
*   **Material:** Housing constructed from a durable, chemically resistant polymer (e.g., Polypropylene). Micro-pillar array encapsulated within a transparent, biocompatible material (e.g. PDMS) for visibility/cleaning.
*   **Power:** Rechargeable lithium-ion battery or inductive charging.

**Pseudocode (Control System):**

```
// Initialize sensors and micro-pillar array

LOOP:
    readViscosity()
    readVolume()
    readUserInput()

    IF userOverride = TRUE THEN
        setApertureShape(userShape)
    ELSE
        // Lookup table/algorithm based on viscosity and volume
        IF viscosity < threshold1 AND volume < threshold1 THEN
            apertureShape = "narrowStream"
        ELSE IF viscosity > threshold2 AND volume > threshold2 THEN
            apertureShape = "wideSpray"
        ELSE
            apertureShape = "default"

        setApertureShape(apertureShape)

    ENDIF

    // Actuate micro-pillars to achieve desired aperture shape
    actuatePillars(apertureShape)
ENDLOOP
```

**Morphology Examples:**

*   **Narrow Stream:** For precise dispensing of low-viscosity liquids (e.g., adhesives, essential oils).
*   **Wide Spray:** For broad coverage of high-viscosity substances (e.g., foams, creams).
*   **Fan Spray:** For even coating of surfaces.
*   **Custom Profiles:** User-defined shapes for specialized applications.

**Potential Applications:**

*   Cosmetics (variable foundation/cream application).
*   Industrial adhesives (precise or broad bonding).
*   Pharmaceutical delivery (controlled droplet size).
*   3D printing (material extrusion control).
*   Culinary arts (sauce application/decorating).