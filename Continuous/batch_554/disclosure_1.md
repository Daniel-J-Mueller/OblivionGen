# 10049589

## Autonomous Obstacle Course Generation for UAV Delivery – ‘Project Nightingale’

**Concept:** Expand the obstacle awareness system to *proactively* create temporary, safe delivery zones by manipulating the environment *before* the UAV arrives, utilizing a network of small, deployable ground-based units.

**Specification:**

**1. Ground Unit – ‘Firefly’:**

*   **Dimensions:** 15cm x 15cm x 10cm.
*   **Locomotion:** Four-wheeled robotic base with independent motor control. Max speed 1m/s.
*   **Payload:**  Lightweight, rapidly deployable, reflective/illuminated barrier segments (approx. 50cm long, foldable).  Each Firefly carries 6 segments.
*   **Sensors:**  LiDAR (range 5m), IMU, proximity sensors (for collision avoidance).
*   **Communication:** Wi-Fi/Bluetooth mesh network.
*   **Power:** Rechargeable battery (2-hour operating time), inductive charging capability.

**2. UAV Integration:**

*   **Mapping:** UAV pre-flight, generates a 3D map of the delivery zone. Identifies static obstacles.
*   **Dynamic Hazard Assessment:**  During flight, identifies *dynamic* obstacles (people, moving vehicles, pets).
*   **Firefly Deployment Instructions:**  Based on hazard assessment, UAV transmits instructions to a swarm of Fireflies to deploy barrier segments, creating a localized, safe landing zone.
*   **Barrier Formation:** Fireflies coordinate to form a circle or square around the designated landing marker, creating a visual and physical buffer zone.
*   **Landing Confirmation:**  UAV confirms the safe landing zone via image analysis, verifying barrier integrity.

**3. Software Architecture (Pseudocode):**

```
// UAV Side
function assess_landing_zone(3D_map, dynamic_obstacles):
  safe_zone = FALSE
  if dynamic_obstacles present:
    request_firefly_deployment(dynamic_obstacles)
    wait_for_deployment_confirmation()
    verify_barrier_integrity()
    if barrier_integrity_confirmed:
      safe_zone = TRUE
  else:
    safe_zone = TRUE
  return safe_zone

function verify_barrier_integrity():
  // Analyze images from UAV cameras to confirm barrier deployment & stability
  // If barrier is compromised, request re-deployment
  return TRUE/FALSE

//Firefly Side
function receive_deployment_instructions(location, barrier_type):
  navigate_to(location)
  deploy_barrier(barrier_type)
  transmit_deployment_confirmation()
```

**4. Operational Modes:**

*   **Autonomous Mode:** UAV automatically initiates Firefly deployment upon identifying dynamic hazards.
*   **Manual Override:** Operator can remotely trigger Firefly deployment or adjust barrier configurations.
*   **Scheduled Deployment:**  Pre-programmed barrier configurations for recurring deliveries to specific locations.

**5. Future Enhancements:**

*   **Variable Barrier Height:**  Deployable barriers with adjustable height for varying levels of protection.
*   **Illumination:** Integrated lighting on barriers for nighttime deliveries.
*   **AI-Powered Hazard Prediction:** Utilize machine learning to anticipate potential hazards and proactively deploy barriers.
*   **Integration with Smart Home Systems:**  Coordinate Firefly deployment with smart home devices to clear delivery zones.