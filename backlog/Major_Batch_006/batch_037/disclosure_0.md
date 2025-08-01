# 9451730

## Variable Stiffness Soft Ducts with Embedded Shape Memory Alloys

**Concept:** Develop soft ducts incorporating embedded Shape Memory Alloy (SMA) wires or meshes to actively control not just cross-sectional area (as in the source patent) but *overall duct stiffness and curvature*. This goes beyond simple constriction/dilation to enable dynamic 3D path adjustment for cooling air delivery.

**Specifications:**

**1. Duct Material:**

*   **Base Layer:** Thermoplastic Polyurethane (TPU) – selected for flexibility, durability, and ease of manufacturing. Durometer: 70A - 85A (adjustable based on application needs).
*   **Reinforcement Layer:**  A woven or knitted fabric comprised of high-elongation polyester or nylon yarns, embedded within the TPU. This provides initial structural integrity.
*   **SMA Integration:**  Nickel-Titanium (NiTi) alloy SMA wires or meshes, pre-stressed and embedded within the reinforcement layer in a grid pattern. Wire diameter: 0.1mm – 0.3mm. Mesh density: Adjustable based on required stiffness control granularity (e.g., 10x10mm to 50x50mm grid spacing).

**2. Activation & Control System:**

*   **Heating Elements:** Micro-resistive heating elements positioned in close proximity to the SMA wires/meshes. Materials: Carbon Nanotube (CNT) composite film for rapid, localized heating.
*   **Temperature Sensors:** Miniature thermocouples embedded within the duct wall to monitor temperature and provide feedback for precise control.
*   **Control Unit:** A microcontroller (e.g., ESP32) with PWM output capabilities to regulate current to the heating elements. Communication: Wireless (WiFi/Bluetooth) for remote control and monitoring.
*   **Power Supply:** Low-voltage DC power supply (5V-12V) – potentially powered over Ethernet (PoE) for ease of deployment in data centers.

**3. Duct Geometry & Manufacturing:**

*   **Manufacturing Method:**  Additive manufacturing (3D printing) using a multi-material printer capable of depositing both TPU and conductive materials for heating elements. Alternatively, a combination of textile weaving/knitting and TPU coating/molding.
*   **Duct Shape:** Initially fabricated as a straight tube, but designed to conform to complex shapes upon SMA activation.
*   **Segmented Control:** The duct is divided into multiple independent segments, each with its own SMA/heating element network, allowing for localized stiffness and curvature control. Segment length: 10cm-30cm.

**4. Operational Pseudocode:**

```
// Function: AdjustDuctSegment(segmentID, targetCurvature, targetStiffness)

function AdjustDuctSegment(segmentID, targetCurvature, targetStiffness) {
  // Read current temperature readings from sensors in segmentID
  currentTemperature = getSegmentTemperature(segmentID);

  // Calculate required heating power based on targetCurvature and targetStiffness
  requiredHeatingPower = calculateHeatingPower(targetCurvature, targetStiffness, currentTemperature);

  // Set PWM duty cycle of heating element for segmentID
  setHeatingElementDutyCycle(segmentID, requiredHeatingPower);

  // Monitor temperature and adjust PWM duty cycle for precise control
  while (true) {
    currentTemperature = getSegmentTemperature(segmentID);
    error = targetTemperature - currentTemperature;
    pwmAdjustment = calculatePID(error);
    setHeatingElementDutyCycle(segmentID, pwmAdjustment);
    delay(10ms);
  }
}

// Main loop
loop {
  // Read desired curvature and stiffness values from control system
  desiredCurvature = getDesiredCurvature();
  desiredStiffness = getDesiredStiffness();

  // Adjust each segment of the duct
  for (i = 0; i < numSegments; i++) {
    AdjustDuctSegment(i, desiredCurvature[i], desiredStiffness[i]);
  }
}
```

**Potential Applications:**

*   **Dynamic Cooling in Data Centers:**  Redirect airflow to hotspots or dynamically adjust cooling based on server load.
*   **Robotics & Flexible Automation:**  Create soft robotic arms or actuators with precise and controllable movement.
*   **Medical Devices:**  Develop minimally invasive surgical tools or flexible endoscopes.
*   **HVAC Systems:**  Improve airflow control and energy efficiency in buildings.