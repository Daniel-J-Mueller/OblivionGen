# 9723072

## Dynamic Network Slice Orchestration via Intent-Based Routing

**Specification:** A system for dynamically creating and managing network slices based on high-level *intent* communicated by the client, rather than specific configuration details. This expands upon the dedicated path concept by allowing for automated creation of complex multi-path slices.

**Components:**

1.  **Intent Engine:** Receives client requests expressed in natural language or a simple declarative language (e.g., “Prioritize low-latency traffic to application X,” or "Isolate perimeter network Y from internal network Z").  Parses intent into a quantifiable set of network requirements (latency, bandwidth, security level, isolation).

2.  **Slice Orchestrator:**  Responsible for translating quantified requirements into a specific network slice configuration. This involves:
    *   **Resource Discovery:** Identifying available resources (bandwidth, compute, endpoint routers, VLANs/MPLS segments) within the provider network.
    *   **Path Computation:**  Calculating optimal paths across the provider network to meet the stated requirements.  This incorporates real-time network conditions (congestion, link failures). Utilizes a graph-based routing algorithm capable of considering multiple metrics simultaneously.  (e.g., Dijkstra’s algorithm modified to support weighted combinations of latency, bandwidth, and security).
    *   **Configuration Provisioning:**  Automatically configuring endpoint routers, VLANs, MPLS segments, and other network devices to create the slice.

3.  **Dynamic Monitoring & Adjustment:**
    *   **Real-time Performance Monitoring:** Continuously monitors the performance of the slice (latency, bandwidth, packet loss).
    *   **Automated Remediation:**  If performance degrades below acceptable levels, the system automatically adjusts the slice configuration (e.g., rerouting traffic, adding bandwidth).
    *   **Predictive Scaling:**  Based on historical usage patterns, the system predicts future bandwidth requirements and proactively scales the slice accordingly.

**Pseudocode (Slice Orchestration):**

```
function createSlice(intent):
  requirements = parseIntent(intent)
  availableResources = discoverResources()
  paths = computePaths(requirements, availableResources)  //Utilize graph-based routing
  if paths == null:
    return "Insufficient resources"
  configuration = generateConfiguration(paths) //Based on paths, generates device configs
  provisionConfiguration(configuration) //Applies config to network devices
  monitorSlice(sliceID) // Starts monitoring process
  return "Slice created successfully"

function provisionConfiguration(configuration):
    for device in configuration:
        applyConfig(device, configuration[device])
```

**Novelty:**

This differs from the provided patent by moving beyond pre-defined dedicated paths and focusing on *dynamic* slice creation driven by high-level intent.  The system doesn't require the client to specify the exact network configuration; instead, it *infers* the configuration from the client’s desired outcome. The addition of predictive scaling and automated remediation provides a more resilient and adaptive network experience. It provides a framework which *creates* the path, instead of requiring it to be established beforehand.