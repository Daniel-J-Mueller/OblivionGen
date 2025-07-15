# 10074320

## Dynamic Sub-Pixel Morphology via Microfluidic Control

**Concept:** Integrate microfluidic channels *within* the sub-pixel structure itself, allowing for dynamic alteration of the sub-pixel’s effective shape and optical properties *during* operation. This goes beyond simply controlling liquid crystal orientation; it alters the physical boundaries of the sub-pixel.

**Specifications:**

1.  **Substrate Layer:** Standard transparent substrate (glass, PET, etc.).
2.  **Microfluidic Layer:**  A layer of etched microfluidic channels embedded *within* the pixel wall structure. Channel dimensions: width 5-20 µm, depth 1-5 µm.  Material: PDMS or similar flexible polymer. Channels should run *parallel* to the pixel wall direction, and *perpendicular* to the sub-pixel width.  A network of these channels forms a ‘mesh’ within each sub-pixel.
3.  **Electrowetting Fluid (EWF) Reservoir:**  A small reservoir integrated *within* each sub-pixel, connected to the microfluidic channel network.  This reservoir contains the EWF.  Reservoir volume: 10-100 nL.
4.  **Actuation Mechanism:** Micro-pumps (piezoelectric, MEMS-based) connected to each sub-pixel’s microfluidic network. Pumps control the flow of EWF within the channels. Control scheme: Precise voltage/current control to modulate pump output.
5.  **EWF Composition:**  Modified EWF containing micro-particles (e.g., reflective or colored particles) suspended within the liquid. Particle size: 0.1-1 µm. Particle concentration: adjustable via software control.
6.  **Transparent Electrode Layer:**  Indium Tin Oxide (ITO) or similar transparent electrode layer patterned on top of the microfluidic layer, forming the electrowetting control electrodes.
7.  **Alignment Layer:** Standard alignment layer for optimal EWF behavior.
8.  **Top Substrate:**  Second transparent substrate to encapsulate the structure.

**Operational Pseudocode:**

```
// For each sub-pixel:

loop {
  // Get target color/brightness value from display controller
  targetColor = getTargetColor();

  // Calculate EWF distribution based on targetColor
  ewfDistribution = calculateEwfDistribution(targetColor); //Algorithm adjusts flow to achieve target

  // Send pump control signals to microfluidic network
  controlMicrofluidicPumps(ewfDistribution);

  // Apply voltage to electrowetting electrodes
  applyElectrowettingVoltage(ewfDistribution);

  // Update display
  updateDisplay();

  delay(1ms);
}
```

**Functionality:**

By controlling the flow of EWF within the microfluidic channels, the effective shape and optical properties of the sub-pixel can be dynamically altered.  This allows for:

*   **Enhanced Color Gamut:** Precise control over particle distribution allows for creating colors beyond the standard RGB spectrum.
*   **Improved Contrast Ratio:** By concentrating particles in specific areas, contrast can be locally enhanced.
*   **Dynamic Sub-Pixel Shaping:**  Creating non-rectangular sub-pixel shapes for optimized viewing angles and reduced moiré effects.
*   **3D Display Effects:** Controlled particle movement can create the illusion of depth.
*   **Adaptive Viewing Angles**: Subpixel shape morphing to compensate for off-axis viewing.