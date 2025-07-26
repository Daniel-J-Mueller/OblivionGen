# 9897793

## Dynamic Partition Wall Height Control for Enhanced Electrowetting Display Performance

**Concept:** The current patent focuses on static partition walls and hydrophobic/hydrophilic layering. This design explores actively adjustable partition wall height *during* display operation. This allows for optimized fluid manipulation, improved contrast ratios, and potential for novel display modes.

**Specifications:**

**1.  Micro-Electro-Mechanical System (MEMS) Partition Walls:**

*   **Material:**  Silicon Nitride (SiN) or Polysilicon.  Chosen for mechanical strength, compatibility with microfabrication processes, and electrical conductivity (for actuation).
*   **Construction:** Each partition wall will be a cantilever structure anchored to the substrate. The base of the cantilever will be electrically conductive, and its height will be controllable via electrostatic actuation.
*   **Actuation:**  Individual electrodes placed *underneath* each partition wall (integrated into the substrate). Applying a voltage between the electrode and a common electrode (or another patterned electrode layer) creates an electrostatic force, bending the cantilever and changing the partition wall height.
*   **Height Range:** Adjustable range of 0-100 μm, with resolution of 1 μm.  Calibration and feedback control required.

**2. Fluid Management Layer:**

*   **Material:**  Thin film polymer with microfluidic channels integrated.
*   **Function:** Distributes the fluids (oil and ethylene glycol) across the display area. Channels are designed to allow fluid flow *around* the adjustable partition walls.
*   **Integration:**  Bonded directly to the substrate *underneath* the adjustable partition walls.

**3. Control System:**

*   **Hardware:**  FPGA-based controller with dedicated DACs (Digital-to-Analog Converters) for precise voltage control of each partition wall electrode.
*   **Software:** Algorithm to map pixel data to partition wall heights.  The algorithm will optimize partition wall height based on factors like viewing angle, ambient light, and desired contrast ratio.
*   **Feedback Loop:**  Capacitive sensors integrated into each partition wall to measure actual height and provide feedback to the controller.

**4. Fabrication Process:**

1.  Deposit and pattern a conductive layer on the substrate to serve as the lower electrode for electrostatic actuation.
2.  Deposit a structural layer (SiN or Polysilicon) and use lithography and etching to define the partition wall structures.
3.  Release the partition walls using a sacrificial layer etching process.
4.  Deposit and pattern the fluid management layer.
5.  Integrate the control electronics and feedback sensors.

**Pseudocode (simplified):**

```
// Function to calculate partition wall height for a given pixel
function calculatePartitionHeight(pixelValue, viewingAngle, ambientLight) {
  // Base height based on pixel value (brightness)
  height = pixelValue * scaleFactor;

  // Adjust height based on viewing angle
  if (viewingAngle > thresholdAngle) {
    height = height * viewingAngleCompensationFactor;
  }

  // Adjust height based on ambient light
  if (ambientLight > thresholdLight) {
    height = height * lightCompensationFactor;
  }

  return constrain(height, minHeight, maxHeight);
}

// Main loop
for each pixel in display {
  height = calculatePartitionHeight(pixelValue, viewingAngle, ambientLight);
  applyVoltage(height, partitionWallElectrode);
}
```

**Novel Aspects:**

*   **Dynamic Control:** Allows for active optimization of fluid behavior, leading to improved contrast, viewing angles, and responsiveness.
*   **Adaptive Display:**  The ability to adjust partition wall height based on environmental factors (viewing angle, ambient light) allows for a more comfortable and immersive viewing experience.
*   **Potential for New Display Modes:** The ability to dynamically shape the fluid droplets opens up the possibility of creating new display effects, such as 3D displays or holographic projections.