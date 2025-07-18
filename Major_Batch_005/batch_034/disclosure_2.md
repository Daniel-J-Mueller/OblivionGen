# 9758313

## Modular Articulated Conveyance System – ‘FlowState’

**System Overview:** A fully modular, dynamically reconfigurable conveyance system utilizing small, independent robotic ‘nodes’ which can attach/detach from a network of rails. This diverges from fixed-angle turns and fixed-path systems.

**Core Components:**

*   **Rail Network:** Standardized rail system, capable of supporting node attachment/detachment. Rails can be linear, curved, or branching. Material: High-strength aluminum alloy.
*   **Conveyance Nodes:** Small, self-powered robotic units (approx. 15cm x 15cm x 10cm). Each node features:
    *   Four omni-directional wheels for movement along rails and free-space.
    *   Electromagnetic attachment mechanism for secure connection to rails and other nodes.
    *   Short-range wireless communication module (Mesh network topology).
    *   Integrated proximity sensors (front, rear, sides).
    *   Small payload platform (max 2kg).
    *   Internal rechargeable battery (inductive charging).
*   **‘FlowState’ Control System:** Centralized software managing node network, path planning, and system monitoring. Includes a drag-and-drop GUI for defining conveyance paths.

**Operational Principles:**

1.  **Dynamic Path Creation:** The 'FlowState' software allows users to define a desired conveyance path by dragging and dropping virtual nodes onto a digital representation of the rail network.
2.  **Node Allocation & Sequencing:** The system automatically allocates physical nodes to the path and sequences their movement to create a ‘virtual conveyor belt’ along the rails.
3.  **Object Transfer:** Items are placed onto the leading node. As the virtual conveyor belt moves, nodes transfer the item sequentially to the next node in the path.
4.  **Adaptive Routing:** The system can dynamically re-route items in response to obstructions or changes in demand. Nodes will autonomously adjust their position and orientation to maintain a continuous flow.
5.  **Off-Rail Capacity:** Nodes can detach from the rail network and move short distances across a flat surface (e.g., a workstation) to deliver items to specific locations.

**Pseudocode (Path Planning Algorithm):**

```
function planPath(startNode, endNode, obstacles):
    path = []
    visited = set()
    queue = [startNode]

    while queue is not empty:
        currentNode = queue.pop(0)

        if currentNode == endNode:
            // Reconstruct Path
            while currentNode is not null:
                path.insert(0, currentNode)
                currentNode = currentNode.previousNode
            return path

        visited.add(currentNode)

        for neighbor in getNeighbors(currentNode):
            if neighbor not in visited and isPathClear(currentNode, neighbor, obstacles):
                neighbor.previousNode = currentNode
                queue.append(neighbor)

    return null // No path found
```

**Specifications:**

*   **Rail Material:** 6061-T6 Aluminum Alloy
*   **Node Dimensions:** 150mm x 150mm x 100mm
*   **Node Weight:** 1.5kg
*   **Max Payload:** 2kg per node
*   **Node Speed:** 0.5 m/s (adjustable)
*   **Communication Protocol:** 802.11ah (Sub-GHz) Mesh Network
*   **Power Supply:** Inductive Charging (wireless)
*   **Control Software:** Python-based GUI with ROS integration.