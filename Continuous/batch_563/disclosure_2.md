# 10218613

## Dynamic Substrate Network Stitching

**Concept:** The patent discusses managing communications *within* a virtual network overlaid on a substrate network. This inspires a system for *dynamically creating* substrate network connections *based on* virtual network traffic patterns. Instead of just routing *over* a static substrate, we *build* the substrate as needed.

**Specs:**

**1. Virtual Network Traffic Monitor (VNTM):**

*   **Function:** Continuously monitors traffic flows *within* the virtual network.
*   **Data Collected:** Source virtual IP, Destination virtual IP, Traffic volume (packets/second, bandwidth), Latency, QoS requirements.
*   **Output:**  Aggregated traffic flow matrix identifying top N virtual IP pairs with highest bandwidth/lowest latency requirements.

**2. Substrate Network Provisioning Engine (SNPE):**

*   **Input:**  Traffic flow matrix from VNTM, available substrate network resources (bandwidth, compute, ports), cost models for different substrate paths.
*   **Algorithm:**
    *   **Phase 1: Path Identification:** For each high-bandwidth/low-latency virtual IP pair, identify potential substrate network paths. This could involve multiple hops, leveraging different network providers.
    *   **Phase 2: Dynamic Provisioning:** If a direct substrate path doesn’t exist or is insufficient, dynamically provision a new path. This could involve:
        *   Software Defined Networking (SDN) control to reserve bandwidth on existing links.
        *   Spinning up virtual network functions (VNFs) as intermediate nodes (e.g., firewalls, load balancers) in the substrate network.
        *   Negotiating bandwidth with different network providers.
    *   **Phase 3: Path Optimization:** Continuously monitor substrate path performance and adjust provisioning based on real-time conditions. This might involve switching between different paths or reallocating bandwidth.
*   **Output:**  A mapping between virtual IP pairs and their corresponding provisioned substrate network paths.

**3. Substrate Network Control Plane (SNCP):**

*   **Function:**  Enforces the substrate network paths defined by the SNPE.
*   **Components:**
    *   **Virtual Tunnel Endpoints (VTEPs):**  Located on the servers hosting the virtual machines. Encapsulate and decapsulate traffic to/from the provisioned substrate paths.
    *   **Flow Steering Logic:**  Directs traffic based on the virtual IP address and the corresponding substrate path.
    *   **Telemetry Reporting:**  Collects performance data on the substrate paths and reports it back to the SNPE.

**4.  Dynamic Adjustment Loop:**

*   **Pseudocode:**

```
LOOP:
  TrafficMatrix = VNTM.getTrafficMatrix()
  Paths = SNPE.provisionPaths(TrafficMatrix)
  SNCP.updatePaths(Paths)
  Telemetry = SNCP.getTelemetry()
  SNPE.optimizePaths(Telemetry)
  WAIT(5 seconds)
END LOOP
```

**Innovation Points:**

*   **Adaptive Substrate:** Creates a substrate network that dynamically adapts to the needs of the virtual network.
*   **Multi-Provider Integration:** Can leverage resources from multiple network providers to create optimal paths.
*   **Automated Provisioning:** Automates the entire process of substrate network provisioning and management.
*   **Reduced Latency:** By creating dedicated paths, reduces latency and improves performance.