# 11250373

## Autonomous Drone Package Retrieval System - "SkyFetch"

**System Overview:** A multi-faceted system utilizing the package computing device (PCD) as a beacon for autonomous drone retrieval *within* the delivery vehicle, and ultimately, for last-mile delivery. This moves beyond simply *locating* a package inside a van, to actively repositioning it for faster, more efficient delivery.

**Hardware Components:**

*   **Enhanced Package Computing Device (ePCD):**  Retains existing light/audio/vibration notification capabilities, but adds:
    *   Ultra-Wideband (UWB) radio for precise indoor positioning (centimeter-level accuracy).
    *   Miniature inertial measurement unit (IMU) – accelerometer & gyroscope – to track package movement/orientation.
    *   Small, shielded RF amplifier for improved UWB signal strength.
    *   Low-power microcontroller for data processing & communication.
*   **Delivery Vehicle Drone (DVD):** Small autonomous drone permanently housed within the delivery vehicle. Equipped with:
    *   Downward-facing UWB receiver to triangulate ePCD location.
    *   Suction/gripper mechanism for package handling (payload capacity matching typical package weights).
    *   Obstacle avoidance sensors (LiDAR/ultrasonic).
    *   Secure docking/charging station within the van.
*   **Vehicle Control Unit (VCU):** Integrates with existing vehicle systems and manages drone operation.

**Software/Firmware Components:**

*   **ePCD Firmware:**  Handles UWB beacon transmission, IMU data capture, and communication with the VCU.
*   **VCU Software:**
    *   Drone flight control & path planning.
    *   UWB signal processing & triangulation.
    *   Package manifest integration (receives package identifiers from remote system).
    *   Collision avoidance algorithms.
    *   User interface for manual override/diagnostic control.
*   **Remote System Integration:** Maintains package tracking information and communicates with both the VCU & the remote computing system.

**Operational Sequence:**

1.  **Package Loading:**  When a package with an ePCD is loaded into the vehicle, the VCU automatically detects its presence via UWB.
2.  **Package Localization:** Throughout the delivery route, the VCU continuously tracks the ePCD location within the van.
3.  **Delivery Optimization:** As the van approaches a delivery location, the VCU calculates the most efficient path for the DVD to retrieve the package.
4.  **Drone Deployment:** The DVD is released from its docking station and autonomously navigates to the package location, using UWB & obstacle avoidance.
5.  **Package Retrieval:** The DVD secures the package with its gripper mechanism.
6.  **Delivery to Drop-off Point:** The DVD either flies the package to a designated drop-off point *on the vehicle itself* (e.g., a rear loading hatch), or to a nearby landing zone for a courier to complete the last-mile delivery.
7.  **Drone Return & Recharge:** The DVD returns to its docking station for recharge and is ready for the next retrieval.

**Pseudocode (VCU – Package Retrieval Function):**

```
FUNCTION RetrievePackage(packageID):
  // Get package location data from UWB triangulation
  packageLocation = GetUWBLocation(packageID)

  // Plan drone flight path to packageLocation, avoiding obstacles
  dronePath = PlanFlightPath(dronePath, packageLocation, obstacleMap)

  // Initiate drone flight
  ActivateDrone(dronePath)

  // Monitor drone progress and adjust path as needed
  WHILE DroneNotAtPackage(packageID):
    UpdateDronePath(dronePath, obstacleData)

  // Command drone to secure package
  SecurePackage(packageID)

  // Return drone to docking station
  ReturnDrone()
END FUNCTION
```

**Novelty:**

This system moves beyond passive package location to *active* package repositioning, reducing courier search time and enabling more efficient delivery. It integrates drone technology *within* the delivery vehicle, creating a mobile robotic retrieval system. The addition of the IMU data to the ePCD allows for package orientation detection, aiding the drone in secure package handling and optimizing the grip. This system also facilitates ‘dynamic load balancing’ by offloading packages to the delivery vehicle exterior. This can reduce the load a courier must carry, increasing their efficiency.