# 9213180

## Dynamic Pixel Shape Control via Microfluidic Channels

**Concept:** Integrate microfluidic channels *within* the electrowetting layer itself, allowing for active shaping of the pixel's viewing area. This moves beyond simply controlling light/dark states and enables dynamic pixel geometries – effectively creating variable resolution displays or displays capable of projecting holographic-like images.

**Specifications:**

1.  **Electrowetting Layer Composition:** The electrowetting layer will consist of two immiscible fluids – a conductive fluid (e.g., water with ionic liquid) and a dielectric fluid (e.g., oil).  Embedded within this layer will be a network of microfluidic channels, fabricated using soft lithography or similar techniques. Channel dimensions will range from 5-50 microns in width.

2.  **Channel Network Architecture:**  Channels will not be uniform across the display. Each pixel will have a dedicated set of microfluidic channels arranged in a hexagonal or Voronoi pattern. This allows for complex deformation of the pixel surface.  Channel layouts will be unique for each pixel to facilitate greater control.

3.  **Actuation Mechanism:**  Each microfluidic channel will have integrated micro-heaters (resistive elements) or piezoelectric actuators. Applying a voltage to these actuators changes the local fluidic pressure within the channel, causing the conductive fluid to move and deform the pixel's surface.

4.  **Addressing Scheme:** A matrix addressing scheme similar to traditional LCDs will be used to control the individual micro-heaters/actuators. This requires a thin-film transistor (TFT) backplane integrated with the base substrate.

5.  **Control Algorithm:**  A software algorithm will map desired pixel shapes (e.g., larger effective area for brighter display, smaller area for sharper focus) to specific voltage patterns applied to the micro-heaters/actuators. This will include feedback loops using optical sensors to measure pixel deformation and adjust the control signals accordingly.

**Pseudocode (Shape Control):**

```
// Input: Desired pixel shape (ShapeData)
// Output: Voltage array for micro-heater array (VoltageArray)

Function CalculateVoltageArray(ShapeData):
  // 1. Convert ShapeData to a deformation map (DeformationMap) representing the desired displacement of each point on the pixel surface
  DeformationMap = ConvertShapeData(ShapeData)

  // 2. Divide pixel surface into finite elements (FE)
  FE = DividePixelIntoElements(DeformationMap)

  // 3. Calculate the force required at each FE node to achieve desired deformation
  ForceArray = CalculateForces(FE, DeformationMap)

  // 4. Calculate the voltage required for each micro-heater to generate the corresponding force
  VoltageArray = CalculateVoltages(ForceArray)

  // 5. Apply a smoothing filter to VoltageArray to reduce artifacts
  VoltageArray = Smooth(VoltageArray)

  Return VoltageArray
```

**Materials:**

*   Base Substrate: Glass or flexible polymer
*   Electrodes: ITO or other transparent conductive material
*   Electrowetting Fluids: Water/ionic liquid and dielectric oil
*   Microfluidic Channels: PDMS or other biocompatible polymer
*   Actuators: Thin-film resistors or piezoelectric materials
*   Encapsulation Layer:  Barrier layer to prevent fluid leakage and environmental degradation.