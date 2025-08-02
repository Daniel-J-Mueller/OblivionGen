# 9182588

## Dynamic Bank Surface Polarity Control

**Concept:**  Instead of *static* hydrophilic/hydrophobic bank surfaces, implement bank surfaces with dynamically adjustable polarity. This allows for far greater control over fluid movement, potentially enabling complex pixel behaviors and reducing voltage requirements.

**Specs:**

1.  **Bank Material:** Utilize a material exhibiting electro-responsive wettability. Ferroelectric polymers (like PVDF-TrFE) or liquid crystal polymers are suitable candidates. These materials change their surface energy (and thus wettability) when an electric field is applied.  The bank should be deposited as a thin film, patterned to define the bank surfaces.

2.  **Bank Structure:** The bank will be built up from multiple layers. A base layer provides structural support.  Above this will be the electro-responsive polymer layer, acting as the dynamically controlled surface. A final, thin passivation layer (e.g., silicon nitride) protects the polymer and ensures consistent performance.

3.  **Electrode Integration:**  Transparent electrodes (ITO or similar) are integrated *within* the bank structure itself, directly adjacent to the electro-responsive layer.  These electrodes, individually addressable, create the electric fields needed to modify the surface wettability. The electrode pattern should allow for localized control – potentially multiple individually controlled segments per bank surface.

4.  **Control System:**  A dedicated driver circuit is required for each pixel.  This driver circuit will:
    *   Receive control signals specifying the desired wettability state for each segment of the bank surface.
    *   Apply the appropriate voltage to the corresponding electrodes within the bank structure.
    *   Implement a feedback loop (using capacitive sensing of the fluid position) to ensure accurate and stable fluid control.

5.  **Pixel Architecture:**
    *   The pixel electrode remains as described in the original patent.
    *   The hydrophobic layer remains as described in the original patent.
    *   The dynamically controlled bank surrounds the hydrophobic fluid, similar to the original patent's concept.

**Pseudocode for Pixel Control:**

```
// Pixel Control Function
function controlPixel(pixelID, bankState) {

  // bankState is an array defining the wettability of each bank segment
  // e.g., [HYDROPHOBIC, HYDROPHILIC, HYDROPHOBIC]

  for (segmentID = 0; segmentID < bankState.length; segmentID++) {

    if (bankState[segmentID] == HYDROPHILIC) {
      applyVoltage(pixelID, segmentID, VOLTAGE_HYDROPHILIC);
    } else { // bankState[segmentID] == HYDROPHOBIC
      applyVoltage(pixelID, segmentID, VOLTAGE_HYDROPHOBIC);
    }
  }
}

// Function to apply voltage to a specific bank segment
function applyVoltage(pixelID, segmentID, voltage) {
  // Access the driver circuit for the specified pixel and segment
  driver = getDriver(pixelID, segmentID);

  // Set the output voltage of the driver circuit
  driver.setOutputVoltage(voltage);
}
```

**Potential Benefits:**

*   **Complex Fluid Control:** Enables non-linear fluid movement and the creation of more intricate pixel shapes/patterns.
*   **Reduced Voltage Requirements:** Dynamically adjusting surface polarity might allow for fluid movement with lower applied voltages.
*   **Improved Response Time:**  Electro-responsive materials can switch wettability states faster than relying solely on the applied voltage across the hydrophobic layer.
*   **Multi-Directional Fluid Movement:**  Allows the fluid to be driven in multiple directions simultaneously within a single pixel.