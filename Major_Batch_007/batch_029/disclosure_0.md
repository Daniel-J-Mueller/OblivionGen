# 9001027

## Dynamic Aperture Control via Micro-Lens Arrays & Electrowetting

**Concept:** Enhance pixel aperture ratio and viewing angle through integration of micro-lens arrays dynamically controlled in conjunction with electrowetting.

**Specifications:**

1.  **Micro-Lens Array Fabrication:**
    *   Material: Transparent polymer (e.g., PDMS, acrylic) with high refractive index.
    *   Lenslet Size: 50-100 μm diameter.
    *   Lenslet Pitch: 100-200 μm (optimized for target viewing angle).
    *   Array Arrangement: Hexagonal close-packing for maximum fill factor.
    *   Fabrication Method: Nanoimprint lithography or UV embossing.

2.  **Electrowetting Integration:**
    *   Each lenslet is positioned *above* a dedicated electrowetting pixel.
    *   Transparent Electrode Layer: ITO or similar, patterned beneath each lenslet.
    *   Dielectric Layer: Thin film dielectric (e.g., SiN) to isolate the transparent electrode from the oil layer.
    *   Oil Layer Composition: Optimized for fast switching and low viscosity.
    *   Electrode Control: Independent control of each lenslet’s underlying pixel electrode.

3.  **Dynamic Control Algorithm:**
    *   Viewing Angle Detection: Integrate a miniature image sensor or utilize existing camera system data to determine user’s viewing angle.
    *   Lenslet Activation Map: Generate a map indicating which lenslets should be ‘activated’ (voltage applied to underlying pixel) based on the detected viewing angle.
    *   Voltage Ramping: Apply a smoothly ramping voltage to activated lenslets to precisely control the curvature and focusing of light.
    *   Refresh Rate: 60-120 Hz refresh rate for smooth visual experience.

4.  **Pseudocode for Dynamic Control:**

```
// Initialization
detectViewingAngle() -> viewingAngle
createLensletActivationMap(viewingAngle) -> activationMap

// Main Loop
while (true) {
    viewingAngle = detectViewingAngle()
    activationMap = createLensletActivationMap(viewingAngle)

    for each lenslet in activationMap {
        if (activationMap[lenslet] == ON) {
            applyVoltage(lenslet, voltageProfile(viewingAngle)) //Voltage profile determines curvature
        } else {
            applyVoltage(lenslet, 0) //Turn off lenslet
        }
    }
}

//Function to define voltage profile based on viewing angle and desired curvature
function voltageProfile(viewingAngle):
    //Define voltage based on angle. Can be refined via AI optimization.
    if (viewingAngle < 30 degrees):
        voltage = 5V
    else if (viewingAngle > 60 degrees):
        voltage = 2V
    else:
        voltage = 3.5V
    return voltage
```

5.  **Panel Stack-up (bottom to top):**
    *   Lower Substrate (Glass/Plastic)
    *   Gate Lines
    *   Pixel Electrodes
    *   Electrowetting Layer
    *   Partition Layer
    *   Oil Layer
    *   Micro-Lens Array
    *   Upper Substrate (Transparent)

6. **Novelty:** Combining precise electrowetting control *with* dynamically adjustable micro-lenses allows for both a maximized aperture ratio *and* improved viewing angles – effectively mitigating the traditional trade-offs in display technology.  This allows for thinner panels with wider viewing cones, and better outdoor visibility.