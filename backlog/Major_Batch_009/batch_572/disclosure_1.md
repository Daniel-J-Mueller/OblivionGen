# 10019029

## Variable Density Haptic Feedback Shell

**Concept:** Integrate micro-actuators *within* the second injection molded layer to create localized variable density/stiffness. This transforms the device housing into a dynamic haptic feedback system. 

**Specifications:**

*   **Material - Second Mold Layer:** Thermoplastic Polyurethane (TPU) infused with micro-capsules containing magnetorheological fluid (MRF).
*   **Actuation:** Array of micro-electromagnets embedded *beneath* the TPU layer. Density of electromagnets will vary based on intended haptic zones.
*   **Control System:**  
    *   Bluetooth Low Energy (BLE) communication with host device (phone, tablet, etc.).
    *   Real-time processing of input data (game events, notifications, UI interactions).
    *   Algorithm to translate input data into individualized current levels for each electromagnet.
    *   Electromagnets alter the viscosity of the MRF, changing localized stiffness/density of the TPU layer. 
*   **Zoning:**  Housing divided into distinct haptic zones (e.g., sides for game controller feedback, back for notification alerts). Zone boundaries defined during manufacturing via localized shielding or varied MRF concentration.
*   **Power:** Integrated solid-state battery (thin-film or similar) positioned within the chassis, rechargeable wirelessly.  Optimized power management to minimize battery drain.
*   **First Mold Layer Integration:**  First layer (high glass fiber content) provides structural rigidity and mounting points for components. Second layer (TPU/MRF) is directly bonded to the first layer during injection molding.
*   **Manufacturing:** Requires multi-stage injection molding process. First layer molded traditionally, then second layer molded *around* embedded micro-electromagnets & wiring.  Automated testing to verify electromagnet functionality and MRF distribution.
*   **Pseudocode – Haptic Feedback Loop:**

```
// Input: Event Data (e.g., game impact, notification type)

function generateHapticPattern(eventData) {
  // Analyze eventData to determine intensity, location, & duration
  intensity = eventData.intensity;
  location = eventData.location;
  duration = eventData.duration;

  // Map intensity to current level for corresponding electromagnets
  currentLevel = map(intensity, 0, 100, 0, 5); // Example mapping

  // Activate corresponding electromagnets
  for (electromagnet in location) {
    electromagnet.activate(currentLevel);
  }

  // Delay based on duration
  delay(duration);

  // Deactivate electromagnets
  for (electromagnet in location) {
    electromagnet.deactivate();
  }
}

loop {
  eventData = receiveEvent();
  generateHapticPattern(eventData);
}
```

*   **Aesthetic Considerations:** Design to seamlessly integrate haptic actuators into the housing aesthetic. Potential for textured surfaces or color-changing materials to further enhance user experience.