# 12254443

## Dynamic Obstacle Field Generation & Collaborative Pathing

**System Overview:**

A distributed system leveraging the existing robot fleet (both item movers & the picking robot) to create a dynamic, real-time obstacle field map. This map isn’t merely about static obstacles, but *predicted* obstacle locations based on fleet movement and item drop probabilities. The picking robot utilizes this predictive map for optimized patrol routes and collision avoidance.

**Hardware Components:**

*   **Existing Robot Fleet:** Each robot acts as a sensor node, broadcasting location & velocity data.
*   **Picking Robot:** Equipped with enhanced LiDAR & ultrasonic sensors for detailed local environment mapping.
*   **Edge Computing Modules:** Each robot (including the picking robot) incorporates a low-power edge computing module for data processing and communication.
*   **Central Server (Optional):**  Used for initial map creation, algorithm updates, and historical data analysis. Communication primarily peer-to-peer.

**Software/Algorithm:**

1.  **Probabilistic Drop Zone Mapping:**
    *   Historical data (item drop locations, robot paths) feeds a machine learning model.
    *   Model predicts ‘drop probability’ for each location in the designated area. Locations with consistently high drop probabilities are marked as ‘potential hazard zones’ with weighted values.
2.  **Dynamic Obstacle Prediction:**
    *   Each robot broadcasts its current location, velocity, and *intended path*.
    *   The picking robot & other robots receive this data.
    *   Using the broadcasted data and predicted drop zones, the picking robot generates a ‘dynamic obstacle field’. This field represents not just current obstacles, but *predicted* future obstacle locations. Obstacles are weighted by drop probability *and* time-to-impact.
3.  **Collaborative Path Planning:**
    *   The picking robot utilizes a modified A* pathfinding algorithm that *incorporates the dynamic obstacle field*.
    *   The algorithm prioritizes paths that minimize exposure to high-weighted obstacles.
    *   Communication between robots allows for *real-time path adjustment*. If one robot detects a drop (or a predicted drop is becoming likely), it broadcasts a warning, triggering path replanning for nearby robots.
4. **Priority-Based Response:**
    * The system identifies dropped items and assigns a priority score based on location (high-traffic areas get higher priority), item type (fragile items get higher priority), and time since drop.
    * This scoring system determines the order in which the picking robot responds to dropped items.

**Pseudocode (Pathfinding with Dynamic Obstacle Field):**

```pseudocode
function A_Star_with_Dynamic_Obstacles(start_node, goal_node, dynamic_obstacle_field):
  open_set = {start_node}
  closed_set = {}

  g_score[start_node] = 0
  f_score[start_node] = heuristic(start_node, goal_node)

  while open_set is not empty:
    current_node = node in open_set with lowest f_score

    if current_node == goal_node:
      return reconstruct_path(current_node)

    open_set.remove(current_node)
    closed_set.add(current_node)

    for neighbor in neighbors(current_node):
      if neighbor in closed_set:
        continue

      tentative_g_score = g_score[current_node] + cost(current_node, neighbor) + obstacle_penalty(neighbor, dynamic_obstacle_field)

      if neighbor not in open_set or tentative_g_score < g_score[neighbor]:
        g_score[neighbor] = tentative_g_score
        f_score[neighbor] = g_score[neighbor] + heuristic(neighbor, goal_node)
        parent[neighbor] = current_node

        if neighbor not in open_set:
          open_set.add(neighbor)

  return failure // No path found

function obstacle_penalty(node, dynamic_obstacle_field):
  penalty = 0
  for obstacle in dynamic_obstacle_field:
    distance = distance_between(node, obstacle)
    if distance < threshold:
      penalty += obstacle.weight / distance //Higher weight, closer obstacle = bigger penalty
  return penalty
```

**Innovation:** Moves beyond simple reactive obstacle avoidance to *proactive* hazard prediction, enabling the picking robot to anticipate drops and optimize its path for increased efficiency & reduced downtime. It's not just reacting to what *is*, but navigating based on what *might be*.