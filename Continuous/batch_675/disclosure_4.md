# 10160569

## Modular Stacking Containers with Active Height Adjustment

**Concept:** Expand upon the variable height nesting concept by creating a modular container system incorporating miniature linear actuators within the support features. This allows for *active* height adjustment of stacked containers *after* they are nested, enabling dynamic shelving and customized organizational solutions.

**Specifications:**

*   **Container Material:** High-impact, recycled polypropylene with reinforcing ribs for structural integrity.
*   **Container Dimensions:** Standard sizes: 12”x12”x8”, 18”x12”x8”, 24”x18”x8”. Customizable dimensions available.
*   **Support Features – Exterior (Container):**
    *   Four primary support pillars – robust, wedge-shaped extensions located at each corner of the container base. Pillars integrate miniature linear actuators (see Actuator Specifications).
    *   Pillar surfaces textured for friction and to prevent rotational slippage.
    *   Integrated proximity sensors within each pillar detect the presence of the container below.
*   **Support Features – Interior (Replica Container):**
    *   Four corresponding recessed cavities, shaped to receive the exterior pillars.
    *   Cavity surfaces lined with compliant material (e.g., silicone) to dampen vibrations and provide a secure fit.
    *   Load cells integrated into the cavity floors measure the weight of the container above.
*   **Actuator Specifications:**
    *   Type: Miniature linear actuator (e.g., piezoelectric or stepper motor-driven lead screw).
    *   Stroke Length: 1-2 inches.
    *   Resolution: 0.1 mm.
    *   Power Source: Low-voltage DC (integrated rechargeable battery with wireless charging capability).
    *   Control: Bluetooth/WiFi connectivity for remote control via smartphone app or integration with smart home systems.
    *   Fail-safe Mechanism: Actuators retract fully in case of power failure to ensure stable stacking.
*   **Control System:**
    *   Smartphone App: Allows users to:
        *   Individually control the height of each container.
        *   Create pre-set height configurations.
        *   Monitor battery levels and actuator status.
        *   Receive alerts for overload or instability.
    *   Smart Home Integration: Compatible with popular smart home platforms (e.g., Alexa, Google Assistant) for voice control and automation.
*   **Power Management:**
    *   Wireless charging base for convenient recharging.
    *   Energy-efficient actuators and control system to maximize battery life.
    *   Optional solar panel integration for off-grid operation.
*   **Safety Features:**
    *   Overload protection: System prevents actuators from exceeding their maximum load capacity.
    *   Stability monitoring: Sensors detect instability and automatically adjust actuator positions to maintain a stable stack.
    *   Emergency stop: Manual override switch to immediately retract all actuators.
*   **Pseudocode (Height Adjustment Logic):**

```
FUNCTION adjustHeight(containerID, targetHeight)
  READ currentHeight FROM containerID
  IF targetHeight > max_height OR targetHeight < min_height THEN
    DISPLAY "Invalid Height"
    RETURN
  ENDIF

  deltaHeight = targetHeight - currentHeight

  FOR EACH actuator IN containerID.actuators
    actuator.extend(deltaHeight)
  ENDFOR

  UPDATE containerID.currentHeight TO targetHeight
END FUNCTION

FUNCTION autoLevel()
  FOR EACH container IN stack
    READ load FROM container.loadCells
    IF load > threshold THEN
      // Adjust actuators to redistribute weight
      // Algorithm to determine optimal actuator extensions
    ENDIF
  ENDFOR
END FUNCTION
```

**Potential Applications:**

*   Dynamic shelving systems for warehouses and retail stores.
*   Customizable storage solutions for homes and offices.
*   Mobile workstations and presentation displays.
*   Automated parts delivery systems in manufacturing plants.
*   Modular furniture and exhibition booths.