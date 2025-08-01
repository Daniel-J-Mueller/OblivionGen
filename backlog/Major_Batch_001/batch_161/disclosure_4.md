# 10120184

**Dynamic Refractive Index Partition Walls**

**Concept:** Instead of simply relying on a static reflective material for the partition walls, integrate microfluidic channels *within* the partition walls, allowing for dynamic control over the refractive index of the wall itself. This leverages the principle that a material's refractive index is directly related to its density and composition.

**Specs:**

*   **Partition Wall Material:** Primarily a transparent polymer (e.g., PDMS, PMMA) with embedded microfluidic channels running vertically through the wall's thickness. Channel dimensions: 50-200um width, 10-50um height. Channel spacing: 200-500um.
*   **Microfluidic Fluid:** A non-conductive, chemically inert fluid with a range of refractive indices achievable through concentration gradients of a high-refractive index nanoparticle (e.g., TiO2, ZrO2) or a different immiscible fluid.
*   **Control System:** An array of micro-pumps and valves (integrated on the display's substrate) precisely controls the flow of the microfluidic fluid within each partition wall's channels. Control is pixel-level.
*   **Refractive Index Range:** Partition wall refractive index adjustable from 1.4 (matching typical transparent fluids) to 1.7 or higher.
*   **Electrowetting Synchronization:** The microfluidic pump/valve system is synchronized with the electrowetting drive circuitry.
*   **Optical Modeling:** Ray-tracing simulations to determine optimal refractive index profiles for maximizing luminance and minimizing color shift at various viewing angles.
*   **Layering:** A thin reflective layer on the *back* of the partition wall (opposite the electrowetting element) to further redirect light.

**Operational Pseudocode:**

```
//Initialization
FOR EACH Partition Wall
    Set Initial Refractive Index to 1.4 //Matching fluid
    Initialize Microfluidic Pump/Valve Array
END FOR

//Frame Update
FOR EACH Pixel
    //Determine optimal partition wall refractive index based on viewing angle and electrowetting state
    Calculate Target Refractive Index = Function(Viewing Angle, Electrowetting State)

    //Adjust microfluidic flow to achieve target refractive index
    Adjust Pump Rate = Function(Target Refractive Index, Current Refractive Index)
    Pump Fluid Through Partition Wall Channels

    //Update electrowetting state (standard operation)
END FOR
```

**Potential Benefits:**

*   **Superior Light Management:** Dynamic control over refractive index allows for precise steering of light within the pixel, maximizing luminance and minimizing internal reflections.
*   **Color Gamut Expansion:** Tailoring the refractive index can compensate for color shifts caused by electrowetting, widening the achievable color gamut.
*   **Viewing Angle Optimization:** Adjusting the refractive index based on viewing angle can improve off-axis performance.
*   **Novel Display Effects:** Creating dynamic light patterns within the pixels.