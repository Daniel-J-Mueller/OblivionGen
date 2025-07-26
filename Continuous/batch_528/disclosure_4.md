# 10557221

## Adaptive Refractive Index Layering for Dynamic Display Clarity

**Concept:** Build upon the optically clear adhesive concept, but introduce multiple layers of adhesives with dynamically adjustable refractive indices to actively compensate for viewing angles and ambient lighting conditions. This creates a display cover that *appears* to have perfect clarity from any vantage point.

**Specifications:**

*   **Cover Material:** Woven fiber base (similar to the original patent) – potentially incorporating conductive fibers for integrated touch sensing.
*   **Layered Adhesive System:**  Minimum of three, maximum of five, optically clear adhesive layers deposited within the woven structure.  Each layer is microfluidically controlled.
*   **Microfluidic Channels:**  Each adhesive layer contains a network of microscopic channels etched into a transparent polymer substrate. These channels allow for precise control of the fluidic composition of the adhesive.
*   **Refractive Index Fluids:**  Two or more fluids with significantly different refractive indices (e.g., 1.4 and 1.6) are pumped through the microfluidic channels. Precise mixing ratios control the effective refractive index of each layer.
*   **Sensor Suite:** Integrated sensors (ambient light, viewing angle via camera/IR tracking) provide real-time data to a control system.
*   **Control System:** A microcontroller processes sensor data and adjusts the fluid pumping rates to optimize the refractive index of each layer, minimizing glare, maximizing contrast, and maintaining apparent clarity.
*   **Power:**  Low-power consumption is crucial.  Potential power sources: inductive charging, integrated thin-film battery, or energy harvesting from ambient light.
*   **Adhesive Properties:** Adhesives must be UV curable and highly transparent.
*   **Layer Thickness:** Each adhesive layer is 5-20 microns thick.

**Pseudocode (Control System):**

```
// Define constants
float REFRACTIVE_INDEX_AIR = 1.0;
float TARGET_REFRACTIVE_INDEX = 1.5; // Default

// Sensor Data Variables
float ambientLightLevel;
float viewingAngleHorizontal;
float viewingAngleVertical;

// Adhesive Layer Control Variables
float layer1MixRatio; // 0.0 - 1.0 (fluid A to fluid B ratio)
float layer2MixRatio;
float layer3MixRatio;

// Function to calculate effective refractive index for a layer
float calculateRefractiveIndex(float mixRatio) {
  return (mixRatio * REFRACTIVE_INDEX_FLUID_B) + ((1 - mixRatio) * REFRACTIVE_INDEX_FLUID_A);
}

// Main Control Loop
while (true) {
  // Read sensor data
  ambientLightLevel = readAmbientLightSensor();
  viewingAngleHorizontal = readHorizontalAngleSensor();
  viewingAngleVertical = readVerticalAngleSensor();

  // Adjust Target Refractive Index based on viewing angle and ambient light
  if (viewingAngleHorizontal > 15 || viewingAngleHorizontal < -15) {
    TARGET_REFRACTIVE_INDEX = 1.55; // Compensate for horizontal viewing
  } else if (ambientLightLevel > 500) {
    TARGET_REFRACTIVE_INDEX = 1.45; // Reduce glare in bright light
  } else {
    TARGET_REFRACTIVE_INDEX = 1.5;
  }

  // Adjust layer mix ratios to achieve target refractive index (simplified example)
  layer1MixRatio = (TARGET_REFRACTIVE_INDEX - 1.4) / 0.2; // Adjust layer 1
  layer2MixRatio = (TARGET_REFRACTIVE_INDEX - 1.45) / 0.15; //Adjust Layer 2
  layer3MixRatio = (TARGET_REFRACTIVE_INDEX - 1.5) / 0.05; //Adjust Layer 3

  //Clamp values
  layer1MixRatio = constrain(layer1MixRatio, 0.0, 1.0);
  layer2MixRatio = constrain(layer2MixRatio, 0.0, 1.0);
  layer3MixRatio = constrain(layer3MixRatio, 0.0, 1.0);

  // Send commands to microfluidic pumps to set mix ratios
  setPumpRatio(1, layer1MixRatio);
  setPumpRatio(2, layer2MixRatio);
  setPumpRatio(3, layer3MixRatio);

  delay(50); // Update rate
}
```

**Potential Applications:** Automotive displays, AR/VR headsets, outdoor signage, high-end smartphone/tablet screens.