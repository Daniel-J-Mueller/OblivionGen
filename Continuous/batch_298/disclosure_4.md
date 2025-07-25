# 9411374

## Dynamic Haptic/Visual Texture Layer

**Concept:** Expand the charged particle display concept to create a dynamic, programmable texture layer *on top* of the primary display. Instead of just color/visual changes, manipulate particle positioning to create tactile sensations alongside the visual output.

**Specifications:**

*   **Layer Stack:**
    *   Primary Display (as described in the provided patent – serves as base).
    *   Transparent, resilient intermediary layer (e.g., a flexible silicone or polyurethane sheet with micro-channels).
    *   Haptic/Visual Particle Layer: Micro-capsules containing charged particles (similar to the FPL, but optimized for both visual & tactile effect). Particle size and material composition are critical – aim for a balance between visual clarity and tactile distinctiveness.
    *   Transparent Top Layer: A durable, scratch-resistant top layer to protect the particle layer and diffuse light evenly.

*   **Electrode Arrangement:**
    *   Dense array of micro-electrodes positioned *below* the haptic/visual particle layer, corresponding to individual or small groups of micro-capsules.
    *   Electrodes capable of precise voltage control – independent control over each electrode is essential.
    *   Electrode material: ITO, carbon nanotubes, or silver nanowire (as mentioned in the patent) are suitable.

*   **Particle Composition:**
    *   Bimodal particle distribution within capsules:
        *   **Visual Particles:** Conventional colored particles for image rendering.
        *   **Haptic Particles:**  Particles with varying stiffness or surface texture (e.g., micro-beads with different coatings).  These particles *do not* necessarily contribute to color.
    *   Particle material: Consider biocompatible polymers, ceramic microspheres, or even micro-fabricated structures.

*   **Control System:**
    *   **Haptic Rendering Engine:** Software module to translate digital textures/shapes into electrode voltage patterns.
    *   **Texture Library:** Predefined library of digital textures (e.g., wood grain, fabric, metal).
    *   **Dynamic Adjustment:**  Algorithm to adjust voltage based on viewing angle (to compensate for parallax) and user touch (force feedback).

**Pseudocode (Haptic Rendering Engine - Simplified):**

```
function renderHapticTexture(textureData, viewingAngle, touchForce):
  for each micro-capsule:
    //Get desired height/position for capsule based on textureData
    capsuleHeight = textureData[capsuleX, capsuleY]

    //Calculate voltage required to achieve capsuleHeight
    voltage = calculateVoltage(capsuleHeight, viewingAngle, touchForce)

    //Apply voltage to corresponding electrode
    setElectrodeVoltage(electrode[capsuleX, capsuleY], voltage)
  end for
end function

function calculateVoltage(capsuleHeight, viewingAngle, touchForce):
  //Base voltage based on capsuleHeight
  voltage = baseVoltage * capsuleHeight

  //Adjust voltage based on viewing angle (parallax correction)
  voltage = voltage + parallaxAdjustment * viewingAngle

  //Adjust voltage based on touch force (feedback)
  voltage = voltage + forceFeedback * touchForce

  return voltage
end function
```

**Potential Applications:**

*   Immersive gaming with realistic tactile feedback.
*   Accessibility for visually impaired users (tactile maps, braille displays).
*   Realistic virtual textures in design and modeling applications.
*   Enhanced user interfaces with customizable tactile buttons and controls.
*   Simulations (surgical training, vehicle operation) with realistic feel.