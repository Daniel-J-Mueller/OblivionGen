# 9086565

**Dynamic Haptic Display Utilizing Ferrofluidic Barriers**

**Concept:** Extend the fluidic control of the disclosed patent to create a dynamic haptic display surface. Instead of just optical switching or image formation, we’ll focus on *tactile* feedback. This builds on the microfluidic control but introduces magnetic fields to shape the fluidic barriers.

**Specifications:**

1.  **Base Layer:** Transparent substrate (glass, acrylic) with an array of micro-reservoirs. Each reservoir contains the “first fluid” (oil) and “second fluid” (water – or a biocompatible equivalent for direct skin contact). Reservoir dimensions: 0.5mm x 0.5mm x 0.1mm. Spacing between reservoirs: 0.1mm.

2.  **Ferrofluidic Barriers:** Instead of relying solely on hydrophilic/hydrophobic surface treatments to define barriers, incorporate a layer of ferrofluid (iron nanoparticles suspended in a carrier fluid) *within* the barrier structure. The ferrofluid layer will be extremely thin (5-10 microns). This ferrofluid will be encapsulated within a micro-channel bordering each reservoir.

3.  **Electromagnetic Control:** Integrate an array of micro-coils *beneath* the substrate, aligned with each micro-channel containing the ferrofluid. Each coil will be individually addressable. When energized, the coils create a localized magnetic field which *shapes* the ferrofluid within the channel, effectively changing the height and permeability of the barrier.

4.  **Tactile Element:** A flexible, transparent membrane (e.g., PDMS) forms the top surface of the display. When a barrier is magnetically raised or lowered, it creates a localized deformation in the membrane, generating a tactile sensation.

5.  **Addressing Scheme:**
    *   Each reservoir is associated with a unique coil.
    *   By precisely controlling the current flowing through each coil, we can modulate the height of the barrier and the resulting tactile sensation.
    *   Pulse Width Modulation (PWM) will be used to create varying degrees of barrier height and thus, varying tactile intensity.

6.  **Fluidic Properties:**
    *   The “oil” (first fluid) should have a high viscosity to provide resistance when the barrier is raised.
    *   The “water” (second fluid) acts as a medium for the oil and should have low surface tension to allow the oil to move easily.
    *   The fluids should be non-toxic and chemically stable.

7.  **Pseudocode (Tactile Pixel Control):**

    ```
    function setTactilePixel(pixel_id, intensity):
        // intensity is a value between 0 and 1 (0 = flat, 1 = maximum height)

        coil_current = intensity * max_coil_current
        activateCoil(pixel_id, coil_current)

    function activateCoil(coil_id, current):
        // Send current to specified coil
        hardware.setCoilCurrent(coil_id, current)
    ```

8.  **Materials:**
    *   Substrate: Glass or Acrylic
    *   Ferrofluid: Commercially available or custom synthesized
    *   Oil: Silicone oil with appropriate viscosity
    *   Water: Deionized water
    *   Membrane: PDMS
    *   Micro-Coils: Copper or other conductive material

9.  **Potential Applications:**
    *   Haptic feedback for VR/AR
    *   Tactile maps for visually impaired individuals
    *   Prototyping physical interfaces
    *   Advanced gaming experiences.