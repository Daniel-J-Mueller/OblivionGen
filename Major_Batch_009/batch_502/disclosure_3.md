# 10001637

## Dynamic Hydrophilicity Control via Embedded Microfluidics

**Concept:** Integrate a microfluidic network *within* the partitioning walls to dynamically alter their surface hydrophilicity, offering per-pixel control beyond simple on/off states and enabling advanced display functionalities.

**Specs:**

*   **Partitioning Wall Material:**  SU-8 or similar photoresist, engineered with microchannels (10-50um diameter) running lengthwise, terminating at each pixel boundary.  Channel density: 2-5 channels per pixel.
*   **Microfluidic Reservoir:**  A network of micro-reservoirs embedded within the substrate beneath the pixel array.  These are filled with two fluids:
    *   **Fluid A:**  Hydrophobic oil (similar to the existing oil layer)
    *   **Fluid B:**  A hydrophilic solution (e.g., water with a surfactant).
*   **Micro-Actuators:** Integrate micro-electromechanical systems (MEMS) valves at the intersection of each microchannel and the reservoir network. Each valve controls the flow of either Fluid A or Fluid B into the channel. Valve control: Individual addressing for each pixel.
*   **Electrode Integration:** Extend the pixel electrodes to encompass sections of the microchannels, allowing for electrowetting-based control of fluid flow *within* the channels, as a secondary actuation method.
*   **Channel Geometry:** Microchannels should have a tapered cross-section. Wider at the reservoir connection, narrowing towards the pixel boundary to maximize capillary force and fluid control.
*   **Hydrophilic/Hydrophobic Coatings:** Internal channel surfaces coated with a permanent hydrophobic layer.

**Operation:**

1.  **Default State:**  All microchannels filled with hydrophobic Fluid A, resulting in the standard electrowetting display operation (hydrophobic partitioning walls, oil layer movement).
2.  **Dynamic Control:**  By selectively activating MEMS valves and/or applying voltage to specific pixel electrodes, Fluid B (hydrophilic solution) can be introduced into individual microchannels. This creates a localized hydrophilic region on the partitioning wall.
3.  **Advanced Display Effects:**
    *   **Gray-Scale Control:**  By controlling the amount of hydrophilic solution in each channel (partial filling), the contact angle of the oil layer can be modulated, enabling gray-scale representation.
    *   **Directional Light Control:**  Creating gradients in hydrophilicity across the partitioning wall can steer the oil droplet in specific directions, enabling anisotropic light reflection and creating unique visual effects.
    *   **Haptic Feedback:**  Modulating the hydrophilicity of the partitioning walls near the surface of the display can create localized changes in surface tension, providing tactile feedback.
    *   **Dynamic Pixel Shaping:** Selective wetting can be used to create dynamically adjustable pixel shapes.

**Pseudocode (Valve Control):**

```
function set_pixel_hydrophilicity(pixel_id, hydrophilicity_level):
  // hydrophilicity_level: 0 (fully hydrophobic) to 1 (fully hydrophilic)

  for each valve in pixel_id's valve array:
    if hydrophilicity_level > 0:
      open valve to allow fluid B flow
      set valve flow rate based on hydrophilicity_level
    else:
      close valve to prevent fluid B flow
      set valve flow rate to 0

//Electrowetting Control - supplementary to valve actuation
function adjust_channel_flow(pixel_id, voltage):
  //voltage: Positive voltage attracts hydrophilic fluid, negative repels.
  apply voltage to electrodes surrounding microchannel of pixel_id
  monitor channel fluid flow based on voltage applied.
```

**Materials:**

*   SU-8 (or similar) for partitioning walls
*   PDMS (Polydimethylsiloxane) for microfluidic channels and reservoirs.
*   Hydrophobic coating (e.g., fluorosilane) for channel interiors.
*   Hydrophilic solution (water + surfactant) & Hydrophobic oil (similar to existing)
*   Transparent electrodes (ITO or similar)
*   Silicon substrate