# 11772908

## Modular, Reconfigurable Robotic Swarm for Item Picking

**System Overview:**

A distributed network of small, wirelessly-communicating robots (“PickBots”) operating within a defined volume. These robots cooperatively identify, lift, and deliver items to designated output locations. The system moves *away* from fixed infrastructure like conveyors and rows of slots, instead utilizing a dynamic, volumetric workspace.

**Hardware Specifications:**

*   **PickBot Dimensions:** 15cm x 15cm x 10cm.
*   **Locomotion:** Four independently driven Mecanum wheels for omnidirectional movement. Maximum speed: 1 m/s.
*   **Lifting Mechanism:** Miniature pneumatic grippers with adjustable pressure settings. Payload capacity: 500g.
*   **Sensors:**
    *   Depth camera (Intel RealSense or similar) for 3D environment mapping and object recognition.
    *   Inertial Measurement Unit (IMU) for precise position tracking.
    *   Proximity sensors (IR or ultrasonic) for collision avoidance.
*   **Communication:** Wi-Fi 6 for low-latency, high-bandwidth communication between PickBots and a central controller.
*   **Power:** Rechargeable Lithium-ion battery. Wireless charging docks distributed throughout the workspace.
*   **Workspace:** Defined volume (e.g., 5m x 5m x 3m). Marked with fiducial markers (AprilTags or similar) to aid in localization.

**Software Architecture:**

*   **Central Controller:** Runs a path planning and task allocation algorithm.
*   **PickBot Software:**  Individual PickBots run a reactive control loop.
*   **Communication Protocol:** A custom message passing system built on UDP.
*   **Task Allocation:** The central controller assigns tasks to PickBots based on proximity to items and available payload capacity.
*   **Path Planning:**  A variant of A* search modified for dynamic obstacle avoidance. The algorithm considers the positions of other PickBots and dynamically adjusts paths to prevent collisions.
*   **Object Recognition:**  A lightweight convolutional neural network (CNN) trained to identify a limited set of target items.
*   **Swarm Coordination:**
    *   **Virtual Force Fields:**  PickBots generate repulsive forces to maintain a minimum distance from each other, preventing collisions.
    *   **Leader Election:**  A distributed algorithm for electing a temporary leader to coordinate complex tasks, such as moving large objects.
    *   **Cooperative Lifting:** Multiple PickBots can work together to lift heavier items by coordinating their movements.
*   **Pseudocode (Cooperative Lifting):**

```
// PickBot A detects heavy object
IF (objectWeight > maxPayload) THEN
  broadcast("request_assistance", objectID, objectWeight)
ENDIF

// PickBot B receives request
ON receive("request_assistance", objectID, objectWeight) DO
  // Calculate optimal grip point
  gripPoint = calculateGripPoint(objectID)

  // Move to grip point
  moveTo(gripPoint)

  // Synchronize lift
  waitFor("lift_signal", objectID)

  // Lift object
  lift(objectID)
ENDIF
```

**Operational Procedure:**

1.  Items are placed randomly within the defined workspace.
2.  The central controller scans the workspace using data from the PickBots’ depth cameras to create a 3D map of the environment and identify the location of all items.
3.  The controller generates a task list, assigning each item to one or more PickBots.
4.  PickBots navigate to their assigned items, grasp them with their pneumatic grippers, and transport them to designated output locations (e.g., packing stations).
5.  Empty PickBots return to charging docks or await new assignments.

**Novelty:**

This system moves away from fixed infrastructure and rigid workflows. It provides a flexible, scalable solution for item picking that can adapt to changing demands and environments. The use of a robotic swarm allows for parallel processing and efficient utilization of workspace volume.