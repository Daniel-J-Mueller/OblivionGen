# 10001639

**Dynamic Pixel Wall Geometry via Microfluidic Control**

**Concept:** Instead of fixed, layered pixel walls, utilize microfluidic channels *within* the support plate to dynamically shape and define pixel boundaries. This allows for variable pixel sizes and shapes – not just rectangular – enabling higher resolution displays with optimized light management.

**Specs:**

*   **Support Plate Material:** Transparent polymer (PMMA, PDMS) with integrated microchannel network. Channels are etched or molded into the plate. Channel dimensions: 5-50µm width, 1-10µm depth.
*   **Fluid:** Electrowetting-compatible, colored dielectric fluid. Viscosity must be controlled for precise flow and containment. Multiple fluids with different colors may be used.
*   **Micro-Actuators:** Integrated micro-pumps/valves (MEMS-based) for controlling fluid flow within the channel network. Actuation controlled by display driver.
*   **Electrode Configuration:** Pixel electrodes *and* microchannel electrodes. Channel electrodes allow for electrostatic steering of the fluid within the channels, sharpening pixel boundaries.
*   **Fluid Containment:** Hydrophobic coating on channel walls to prevent fluid leakage.
*   **Layer Stack:**
    1.  Bottom Support Plate (with integrated microfluidic network and electrodes)
    2.  Hydrophobic Layer
    3.  Pixel Electrodes
    4.  Electrowetting Oil
    5.  Electrolyte Solution
    6.  Top Support Plate
*   **Control System:**
    *   Display Driver sends commands to micro-pump/valve array.
    *   Array controls fluid flow within microchannels, defining pixel boundaries.
    *   Channel electrodes modulate fluid shape for improved pixel definition.
*   **Pixel Addressing:** Each pixel is defined by a unique combination of microchannel activation and electrode modulation.
*   **Pseudocode:**

```
// Pixel activation function
function activatePixel(pixelID, color) {
  // Get microchannel array associated with pixelID
  channelArray = getChannelArray(pixelID);

  // Set channel pumps to fill channels with desired color fluid
  for (channel in channelArray) {
    setPumpState(channel, ON);
    setFluidColor(channel, color);
  }

  // Activate channel electrodes to sharpen pixel boundary
  activateElectrodes(pixelID);
}

// Electrode activation function
function activateElectrodes(pixelID) {
  // Calculate electric field required to sharpen boundary
  electricField = calculateElectricField(pixelID);

  // Apply electric field to channel electrodes
  applyElectricField(pixelID, electricField);
}
```

**Innovation:** This system moves beyond static pixel wall structures. Allows for dynamic pixel shaping (circles, triangles, arbitrary shapes) improving display aesthetics and potentially reducing light leakage by optimizing pixel packing density. Also enables novel display effects like variable pixel resolution (higher resolution in areas requiring detail) and dynamic light management.