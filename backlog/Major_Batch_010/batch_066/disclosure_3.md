# 11388834

## Dynamic Power Allocation via Wireless Energy Transfer

**Concept:** Integrate wireless energy transfer (WET) into the server power distribution system, enabling dynamic power allocation to components based on real-time need, and facilitating hot-swapping/reconfiguration without physical power connection/disconnection.

**Specs:**

*   **WET Transmitter Modules:** Small form-factor WET transmitter modules mounted within the server chassis – likely alongside or integrated into the existing PDB mounting locations. These modules utilize resonant inductive coupling to transfer power wirelessly. Frequency: 6.78 MHz (or similar ISM band frequency optimized for efficiency and safety within the server environment). Output Power: Scalable from 1W to 50W per module. Modules operate under digital control.
*   **Receiver Coils/Rectifiers:** Receiver coils integrated *directly* into or onto the power input connectors/pads of server components (CPUs, GPUs, storage drives, network cards, etc.).  These coils are paired with rectifier circuits converting the received RF energy back into DC voltage suitable for the component. Receiver coil size/design optimized for each component's power requirements and physical space.
*   **Power Management Controller (PMC):** A central PMC monitors component power consumption *in real time*.  The PMC controls the output power of each WET transmitter module, dynamically adjusting it to match each component’s demand. Algorithm: Proportional-Integral-Derivative (PID) control loop for stable and responsive power delivery.  Predictive algorithms leverage historical usage data to anticipate power needs.
*   **Communication Protocol:** Wireless communication (e.g., Bluetooth Low Energy, Zigbee) between the PMC and each component’s receiver circuit. This allows for bi-directional communication: PMC sends power commands; receiver reports current draw, voltage, and temperature.
*   **Safety Mechanisms:**
    *   **Foreign Object Detection (FOD):**  Detects metallic objects near the WET transmitter coils to prevent overheating or damage.
    *   **Over-Current/Over-Voltage Protection:**  Integrated into both the transmitter and receiver circuits.
    *   **Thermal Monitoring:**  Temperature sensors monitor both transmitter and receiver coils to prevent overheating.
    *   **Emergency Shutdown:** A dedicated shutdown circuit cuts power to all WET transmitters in case of a detected fault.
*   **PDB Integration:** Existing PDB retains its function as the primary power source for components *not* utilizing WET.  The PDB also provides power to the WET transmitter modules. A software interface allows the system administrator to selectively enable or disable WET for individual components.
*   **Component Power Input Pads:**  Standardized power input pads on server components, designed to accommodate both traditional power connections *and* inductive coupling.  The pads will include alignment features to maximize coupling efficiency.

**Pseudocode (PMC Control Loop):**

```
FOR each component in component_list:
    current_power_demand = read_power_demand(component)
    target_power = calculate_target_power(current_power_demand)
    error = target_power - actual_power_delivered(component)
    adjustment = PID_controller(error)
    set_transmitter_power(component, adjustment)
END FOR

//Background Task:
//Monitor temperature, FOD, and safety thresholds.
```

**Innovation Rationale:** This system moves beyond static power distribution, allowing for greater energy efficiency, simplified component reconfiguration, and potential for hot-swapping components without interrupting system operation.  It opens the door to more flexible and dynamic server designs.