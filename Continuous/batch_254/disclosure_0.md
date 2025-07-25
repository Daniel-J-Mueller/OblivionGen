# 9489655

## Robotic Swarm Localization via Dynamic RFID Constellations

**Concept:** Extend the core RFID tracking system to support localization of a swarm of robotic pickers within a warehouse, using a dynamically reconfigurable 'constellation' of RFID readers and tags. Instead of relying on a single reader/picker pairing, create a mesh network where each picker carries multiple RFID tags, and strategically placed, mobile RFID readers (mounted on smaller automated guided vehicles - AGVs) act as 'stars' in a constellation.

**Specs:**

*   **Picker Hardware:**
    *   Each robotic picker is equipped with 6 RFID tags, positioned around its perimeter (top, bottom, front, back, left, right). Tag identifiers are uniquely assigned and known by a central control system.
    *   Each tag utilizes a high-frequency (UHF) RFID chip with adjustable transmission power (10mW - 1W).
    *   Picker includes a low-power wide-area network (LoRaWAN) module for communication with the central control system.
*   **Mobile RFID Reader AGVs:**
    *   10-20 AGVs, each equipped with 3-4 phased array RFID readers capable of beamforming.
    *   Each reader can scan in a 360-degree radius up to 15 meters.
    *   AGVs navigate autonomously using a separate localization system (e.g., LiDAR SLAM), but report their position to the central control system.
    *   AGVs utilize onboard processing for initial signal analysis (RSSI, phase of signal).
*   **Central Control System:**
    *   Receives data from both the robotic pickers (LoRaWAN) and the mobile RFID reader AGVs (WiFi).
    *   Implements a trilateration algorithm, but dynamically weighted based on signal strength and phase information.
    *   Utilizes a Kalman filter to smooth position estimates and predict future movement.
    *   The system maintains a dynamic map of RFID signal strengths throughout the warehouse.

**Algorithm (Pseudocode):**

```
// Initialization
dynamicMap = createEmptyWarehouseMap()
pickerLocations = {}

// Real-time Loop

for each AGV:
    scanRFIDTags()
    for each tag detected:
        tagID = getTagID()
        RSSI = getReceivedSignalStrength()
        phase = getSignalPhase()

        //Update dynamic map
        dynamicMap.update(tagID, AGV.location, RSSI, phase)

//For each picker
for each picker:
    pickerID = picker.ID
    potentialLocations = dynamicMap.getPossibleLocations(pickerID)
    bestLocation = trilateration(potentialLocations, RSSI, phase) //Weighted trilateration

    //Kalman Filter
    estimatedLocation = kalmanFilter(bestLocation, picker.lastKnownLocation, picker.velocity)

    picker.lastKnownLocation = estimatedLocation
```

**Novelty:**

*   **Dynamic Constellation:** Unlike static RFID deployments, this system leverages a mobile mesh network, increasing coverage and robustness.
*   **Phase-Based Localization:** Utilizing the phase of the RFID signal alongside RSSI provides more accurate distance estimates, reducing ambiguity.
*   **Swarm Intelligence:** Multiple pickers and AGVs collaboratively create a more reliable and scalable localization system.

**Potential Applications:**

*   High-density warehouse environments.
*   Real-time inventory management.
*   Autonomous robot navigation.
*   Improved pick accuracy and efficiency.