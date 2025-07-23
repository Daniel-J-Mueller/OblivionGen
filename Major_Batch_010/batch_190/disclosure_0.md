# D678048

## Modular Carton System with Integrated Haptic Feedback

**Concept:** A carton system comprised of interlocking, geometrically modular units. Each unit contains embedded haptic feedback elements, allowing for dynamic textural changes on the carton's exterior. 

**Specs:**

*   **Base Unit:** Hexagonal prism, 5cm side length, constructed from recycled polypropylene.
*   **Interlocking Mechanism:** Magnetic connectors embedded within each face of the prism, allowing for 360-degree rotational and translational connection to adjacent units. Connector strength: 2N.
*   **Haptic Elements:** Array of miniature, shape-memory alloy (SMA) actuators embedded beneath the surface of each hexagonal face. Actuator density: 10 actuators/cm².
*   **Actuator Control:** Each actuator independently controllable via Bluetooth Low Energy (BLE). Resolution: 256 levels of actuation force/texture.
*   **Power Source:**  Ultra-thin flexible battery integrated within the base of each unit. Capacity: 100mAh. Rechargeable via inductive charging.
*   **Surface Material:**  Flexible TPU coating over the SMA actuators, allowing for localized deformation and texture generation.
*   **Software Interface:** Mobile app for custom texture design and control. Functionality includes pre-set textures (e.g., wood grain, fabric, embossed patterns), dynamic texture animation, and haptic feedback triggered by external stimuli (e.g., sound, touch).
*   **Unit Variants:**
    *   *Corner Unit:*  Hexagonal prism with angled faces for creating curves and corners.
    *   *Edge Unit:*  Elongated hexagonal prism for building linear structures.
    *   *Flat Unit:*  Thin hexagonal plate for creating flat surfaces.
*   **Assembly Protocol:** Units connect via magnetic force, ensuring easy assembly and disassembly. The software recognizes the assembled structure and allows for targeted haptic feedback control.
*   **Pseudocode (Texture Generation):**

```
// Define texture parameters
textureType = "woodGrain"
grainDirection = 45 degrees
grainHeight = 2mm
frequency = 5Hz

// Iterate through each actuator
for each actuator in actuatorArray:
  // Calculate actuator displacement based on texture parameters and actuator position
  displacement = calculateDisplacement(textureType, grainDirection, grainHeight, frequency, actuator.x, actuator.y)

  // Set actuator force
  actuator.force = map(displacement, 0, maxDisplacement, 0, maxForce)
  actuator.actuate()
end for
```

**Application:** Packaging, interactive displays, modular furniture, educational toys. The system allows for customizable and dynamically changing packaging, providing a novel user experience and enhanced branding opportunities.