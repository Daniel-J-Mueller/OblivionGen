# 11222299

## Autonomous Vehicle Swarm Mapping & Dynamic Obstacle Negotiation

**System Overview:** A distributed system leveraging a swarm of small, low-cost autonomous vehicles (“ScoutBots”) to create and maintain real-time, high-fidelity 3D maps of interior spaces, specifically focusing on dynamic obstacle identification and avoidance *beyond* what a single robot could achieve. This goes beyond simple SLAM and aims for *predictive* obstacle negotiation.

**Hardware Components (ScoutBot):**

*   **Dimensions:** 15cm x 15cm x 10cm
*   **Locomotion:** Four-wheeled differential drive.
*   **Sensors:**
    *   LiDAR (360-degree, short-range, 5m max)
    *   Depth Camera (Stereo vision, 3m max)
    *   IMU (Inertial Measurement Unit)
    *   Microphone Array (for sound source localization)
    *   Low-Resolution Wide-Angle Camera (for visual context – color, texture)
*   **Compute:** Edge TPU for onboard processing.
*   **Communication:** Wi-Fi 6 / Bluetooth 5.2 Mesh Networking.
*   **Power:** Solid-state batteries, inductive charging.

**Software Architecture:**

1.  **Decentralized Mapping:** Each ScoutBot independently performs SLAM (Simultaneous Localization and Mapping). Instead of a central map, ScoutBots exchange *local map updates* and *obstacle detections* with neighboring bots.  This creates a constantly refining, distributed map.

2.  **Predictive Obstacle Modeling:** 
    *   **Sound Source Localization:** The microphone array detects sounds (e.g., voices, moving objects) to identify potential obstacles *before* they are visually detected.
    *   **Motion Prediction:** Based on observed movement patterns (from LiDAR and Depth Camera), an LSTM (Long Short-Term Memory) neural network predicts the future trajectory of moving obstacles.
    *   **Obstacle Classification:** Obstacles are classified (e.g., person, chair, box) using computer vision techniques.

3.  **Dynamic Path Planning:** 
    *   **Distributed A* Algorithm:** A modified A* path planning algorithm is implemented in a distributed manner. Each ScoutBot contributes to the path planning process by evaluating potential paths based on local map information and predicted obstacle movements.
    *   **Negotiation Protocol:** ScoutBots communicate and negotiate paths to avoid collisions and optimize overall swarm efficiency.  This involves exchanging information about intended routes, obstacle predictions, and confidence levels.

4.  **Hierarchical Swarm Control:**
    *   **Leader Election:** A leader election algorithm dynamically selects a leader ScoutBot based on factors such as battery life, processing power, and map coverage.
    *   **Task Allocation:** The leader ScoutBot assigns tasks (e.g., mapping specific areas, monitoring obstacle movements) to other bots.

**Pseudocode (Distributed Path Planning):**

```
// ScoutBot i receives a destination from the central system
Destination = receiveDestination()

// Generate a set of candidate paths using local map data
CandidatePaths = generateCandidatePaths(Destination, LocalMap)

// Predict obstacle movements along each path
For Each Path in CandidatePaths:
    ObstaclePredictions = predictObstacleMovements(Path)

// Evaluate each path based on predicted obstacle movements and path cost
For Each Path in CandidatePaths:
    PathCost = calculatePathCost(Path, ObstaclePredictions)

// Communicate PathCost and ObstaclePredictions to neighboring ScoutBots
broadcast(PathCost, ObstaclePredictions)

// Receive PathCost and ObstaclePredictions from neighboring ScoutBots

// Combine local and received information to refine path evaluation
CombinedPathEvaluations = refinePathEvaluations(localPathEvaluations, receivedPathEvaluations)

// Select the optimal path based on combined evaluations
OptimalPath = selectOptimalPath(CombinedPathEvaluations)

// Transmit intended path to neighboring ScoutBots
broadcast(OptimalPath)

// Monitor for path conflicts and adjust as needed
monitorPathConflicts()
adjustPath()
```

**Application:**

*   **Autonomous Delivery in Dynamic Environments:** The system can be used to deliver items in crowded environments (e.g., hospitals, offices) where obstacles are constantly moving.
*   **Real-Time Mapping and Monitoring:** The system can create and maintain up-to-date maps of interior spaces for security and surveillance purposes.
*   **Search and Rescue:** The swarm of ScoutBots can be deployed to search for missing persons in complex environments.
*   **Infrastructure Inspection:** The ScoutBots can be used to inspect critical infrastructure (e.g., pipes, cables) for damage or defects.