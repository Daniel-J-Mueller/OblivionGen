# 10001637

## Dynamic Hydrophobicity Control via Microfluidic Integration

**Concept:** Integrate microfluidic channels *within* the partitioning walls of the electrowetting display to dynamically alter the hydrophobicity of specific pixels, creating a grayscale effect *without* voltage control, and enabling more complex color mixing.

**Specs:**

*   **Partitioning Wall Material:** SU-8, as described in the patent, but with incorporated microchannels (500nm - 1um diameter) running longitudinally along the height of the wall. These channels should not compromise structural integrity.
*   **Microfluidic Fluid:** A non-conductive fluid with tunable surface tension/hydrophobicity, potentially using a surfactant concentration gradient or a fluid comprised of micro/nanoparticles with controlled wetting properties.
*   **Channel Network:** Each pixel's partitioning walls will contain dedicated microfluidic channels connected to a centralized micro-pump/reservoir system.
*   **Control System:** A system for precise control of fluid flow within each pixel’s partitioning walls. This could be implemented through a network of micro-valves or digitally addressable micro-pumps.
*   **Electrode Design:** Maintain existing pixel electrode design.  The oil layer and water droplets will still form between the electrodes and partitioning walls.

**Operation:**

1.  **Baseline:** Initially, all microfluidic channels contain the base fluid, maintaining consistent hydrophobicity and normal electrowetting display operation.
2.  **Grayscale/Color Control:** To achieve grayscale or color variation, the control system pumps varying concentrations of the wetting/non-wetting fluid into the microchannels of specific partitioning walls.
3.  **Hydrophobicity Gradient:** Increasing the concentration of the wetting fluid in a microchannel *increases* the hydrophobicity of that partitioning wall’s surface.  This alters the contact angle of the water droplet, effectively modulating the pixel’s perceived brightness or color.  A higher contact angle ‘pulls’ the droplet away from the electrode, reducing capacitance and thus perceived brightness.
4.  **Dynamic Adjustment:** The fluid concentration and thus hydrophobicity of each pixel can be dynamically adjusted in real-time, enabling smooth grayscale transitions and color mixing without relying on voltage modulation.

**Pseudocode (Control Algorithm):**

```
// Inputs: Pixel Coordinates (x, y), Desired Brightness (0-100)
// Outputs: Microfluidic Pump Control Signals (Flow Rate for each pixel)

function controlPixelBrightness(x, y, desiredBrightness) {

    // Calculate target flow rate based on desiredBrightness
    targetFlowRate = map(desiredBrightness, 0, 100, minFlowRate, maxFlowRate);

    // Send control signal to microfluidic pump for pixel (x, y)
    setPumpFlowRate(x, y, targetFlowRate);

}

// Example of dynamic adjustment
loop {

  for each pixel (x, y) in display {

    // Calculate brightness based on some input (e.g., image data)
    brightness = getImageBrightness(x, y);

    // Control pixel brightness
    controlPixelBrightness(x, y, brightness);

  }

  // Update display
  updateDisplay();

  delay(10ms); // Adjust delay as needed
}
```

**Potential Benefits:**

*   **Simplified Drive Electronics:** Reduces or eliminates the need for complex and high-precision voltage control circuits.
*   **Enhanced Contrast & Grayscale:**  Offers finer control over pixel brightness, resulting in higher contrast and smoother grayscale transitions.
*   **New Display Modes:**  Enables the creation of novel display modes beyond traditional electrowetting, leveraging dynamic hydrophobicity control.
*   **Potential for Color Mixing:** By individually controlling the hydrophobicity of partitioning walls around different colored droplets, localized color mixing and effects could be achieved.