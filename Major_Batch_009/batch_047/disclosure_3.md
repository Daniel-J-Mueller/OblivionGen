# 11091355

## Adaptive Conformable Gripper - Bio-Inspired Pneumatic Actuation

**Concept:** A soft robotic gripper employing multiple, independently controlled pneumatic chambers arranged in a bio-inspired, tentacle-like structure. The design prioritizes adaptability to irregular object shapes and fragility through distributed sensing and localized pressure control.

**Specs:**

*   **Structure:** A main body constructed from a flexible, durable elastomer (e.g., silicone). Embedded within are multiple (16-64) small, independently addressable pneumatic chambers. These chambers are arranged in a branching, tentacle-like configuration, extending outward from the central housing.
*   **Chamber Design:** Each chamber is a sealed cavity with a flexible membrane on its exterior surface.  Internal volume: 0.5 - 2 mL. Membrane material: Thin polyurethane film with integrated strain sensors.
*   **Actuation System:** A microfluidic control system using miniature solenoid valves. Each valve controls the flow of compressed air to an individual chamber.  Pressure range: 0 - 150 kPa. Response time: < 50ms. Air supply: External compressed air source (40-60 PSI).
*   **Sensing:** Each chamber's membrane incorporates piezoresistive or capacitive strain sensors. These sensors measure the deformation of the membrane, providing feedback on the applied pressure and contact force. Data is transmitted wirelessly via Bluetooth Low Energy (BLE) to a central controller.
*   **Control System:** A microcontroller (e.g., ESP32) processes sensor data and controls the solenoid valves.  Algorithms implement:
    *   **Adaptive Grasping:** Based on object shape and material properties (estimated from sensor data), the system adjusts the pressure in individual chambers to conform to the object.
    *   **Force Control:** Limits the maximum force applied by each chamber to prevent damage to fragile objects.
    *   **Slip Detection:** Monitors sensor data for sudden changes indicating slippage and adjusts grip accordingly.
*   **External Interface:** ROS (Robot Operating System) compatible API for integration with higher-level robotic control systems.
*   **Materials:**
    *   Elastomer: Silicone rubber (Shore A hardness 40-60)
    *   Membrane: Polyurethane film (50-100 μm thickness)
    *   Solenoid Valves: Miniature proportional solenoid valves
    *   Sensors: Piezoresistive or capacitive strain sensors
*   **Dimensions:** Overall length: 150-300 mm. Diameter: 50-100 mm.
*   **Power Requirements:** 5V DC, < 500mA.

**Pseudocode (Grasping Sequence):**

```
FUNCTION graspObject(objectShape, objectMaterial):
  // 1. Initial Approach: Move gripper towards object
  moveGripper(objectPosition)

  // 2. Contact Detection: Activate proximity sensors
  IF proximitySensorTriggered():
    // 3. Shape Estimation: Use sensors to estimate object shape
    objectShape = estimateShape()

    // 4. Chamber Activation: Activate chambers based on shape
    FOR each chamber IN chamberList:
      IF chamberContactPoint(chamber, objectShape):
        activateChamber(chamber, pressureMap(chamber, objectShape, objectMaterial))

      ELSE:
        deactivateChamber(chamber)

    // 5. Force Control: Monitor sensor data and adjust pressure
    WHILE objectNotSecurelyGrasped():
      adjustChamberPressure(sensorData, desiredForce, objectMaterial)

  ELSE:
    //Object Not Found
    moveGripper(homePosition)
```

**Novelty:** Existing soft grippers often rely on uniform inflation or pre-defined grasping patterns. This design prioritizes *localized* and *adaptive* pressure control based on real-time sensor feedback, enabling it to handle a wider range of objects, particularly those with irregular shapes or delicate surfaces. The bio-inspired tentacle-like structure allows for increased reach and manipulation dexterity.