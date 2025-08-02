# 11084666

## Dynamic Sortation Floor with Modular Transfer Units

**Concept:** Introduce a sortation floor comprised of independently addressable, modular transfer units (MTUs) capable of both horizontal and *vertical* movement. These MTUs form the sortation surface, and can dynamically raise or lower to create pathways, blockages, or elevated destinations for packages. Overdrive units, as described in the referenced patent, would operate *above* this dynamic surface.

**Specifications:**

*   **MTU Dimensions:** 60cm x 60cm x 15cm (L x W x H). Standardized dimensions for modularity.
*   **MTU Actuation:** Each MTU utilizes a linear actuator (electric or pneumatic) providing 10cm of vertical travel. Travel speed: 5cm/s.
*   **MTU Surface:** Durable, low-friction composite material. Integrated RFID tag for identification and positioning.
*   **MTU Power/Communication:** Wireless power transfer (inductive charging) & wireless communication (WiFi 6) for minimal cabling.
*   **Floor Configuration:** MTUs are arranged in a grid pattern. Grid size scalable based on facility requirements.
*   **Overdrive Unit Interface:** Overdrive units are equipped with downward-facing sensors (LiDAR or ultrasonic) to detect MTU height and adjust accordingly. Minimum clearance: 5cm between overdrive unit and MTU surface.
*   **Control System:** Centralized controller with real-time MTU positioning data. Algorithm for path planning, collision avoidance, and dynamic pathway creation.
*   **Safety Features:** Emergency stop mechanism. Obstacle detection sensors on both MTUs and overdrive units.

**Operational Pseudocode:**

```
// Package arrives at induct station
PackageData = ReceivePackageData(PackageID, Destination)

// Controller determines optimal path
Path = CalculateOptimalPath(PathID, Destination)

// Controller instructs MTUs to create pathway
FOR EACH MTU in Path:
    SetMTUHeight(MTU_ID, Height = 5cm) //Raise MTU to create path
END FOR

// Overdrive Unit dispatched
DispatchOverdriveUnit(OverdriveUnitID, PathID, PackageData)

// Overdrive Unit traverses path
WHILE OverdriveUnit traveling:
    MonitorMTUHeights(PathID) //Ensure pathway remains clear
    AdjustOverdriveHeight(RealtimeMTUHeights)
END WHILE

// Overdrive Unit arrives at destination
SetMTUHeight(DestinationMTU_ID, Height = 0cm) // Lower destination MTU

// Package transfer initiated
TransferPackage(OverdriveUnitID, DestinationMTU_ID, PackageData)

// Overdrive Unit returns to induct station
ReturnOverdriveUnit(OverdriveUnitID)
```

**Novelty:**

This design differs from the referenced patent by introducing a dynamic sortation surface *underneath* the overdrive units. Instead of relying solely on the movement of overdrive units and container drive units, the floor itself actively participates in the sorting process. This enables:

*   **Increased Sort Density:** The ability to create temporary pathways and elevated destinations allows for more efficient use of floor space.
*   **Dynamic Re-routing:** Packages can be re-routed mid-sort without requiring container drive units to physically move.
*   **Reduced Congestion:** The ability to create multiple simultaneous pathways minimizes bottlenecks.
*   **Adaptability:** The dynamic floor can be reconfigured to accommodate changing sortation requirements.