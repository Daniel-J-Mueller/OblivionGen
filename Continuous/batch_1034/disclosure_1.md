# 8472183

## Modular Liquid Cooling Distribution Manifold

**Concept:** A rack-mounted unit integrating a liquid cooling distribution manifold *within* the power supply unit (PSU) housing, utilizing the PSU’s existing airflow pathways to enhance heat exchange. This moves liquid cooling closer to the primary heat sources (CPU, GPU, VRMs) and simplifies rack-level liquid cooling implementation.

**Specifications:**

*   **Form Factor:** Standard ATX or SSI-EEB PSU size, designed to be physically interchangeable with standard PSUs.
*   **Liquid Cooling Block Integration:** Integrated cold plate(s) directly mounted to the PSU’s internal heat sinks and potentially covering key components like the switching regulators and transformers.
*   **Manifold Design:** Internal microchannel manifold constructed from copper or nickel-plated copper. Channels are designed for low-restriction flow.
*   **Inlet/Outlet Ports:** Standard G1/4” threaded ports for liquid cooling loop connections. Positioned for compatibility with standard quick-disconnect fittings.
*   **Pump Integration:** Option for integrated micro-pump (low-flow, high-head) within the PSU housing. Can be bypassed if external pump is used.
*   **Flow Monitoring:** Integrated flow sensor to monitor coolant flow rate. Data reported via standard IPMI/Redfish or USB.
*   **Leak Detection:** Integrated leak sensor with automatic shutdown capability.
*   **Fan Integration:** Uses existing PSU fans, or provides dedicated fan mounting points for increased airflow across the cooling block. Fan speed controllable via PWM.
*   **Power Supply Compatibility:** Designed to work with standard ATX power supplies, or with dedicated high-efficiency power supplies optimized for liquid cooling.
*   **Materials:** PSU housing constructed from aluminum or steel. Internal components constructed from copper, nickel-plated copper, and chemically inert polymers.
*   **Software Interface:**  Basic monitoring & control interface. Fan speed control, flow rate monitoring, leak detection alerts. Interface accessible via standard IPMI/Redfish or USB.

**Operational Pseudocode:**

```
// Initialize System
IF (Leak Detected) THEN Shutdown System;
Initialize Fan Control;
Initialize Flow Sensor;

// Main Loop
WHILE (System Running) DO
    Read Temperature Sensors (CPU, GPU, PSU Components);
    Read Flow Rate Sensor;
    Calculate Cooling Demand;
    Adjust Fan Speed (based on Cooling Demand & Temperature);
    Monitor for Leaks;
    Report System Status (Temperature, Flow Rate, Leak Status) via IPMI/USB;
END WHILE
```

**Rack Integration:**

*   Standard 1U or 2U rack mounting.
*   Units designed to be daisy-chained for larger rack configurations.
*   Coolant distribution manifold connects to external liquid cooling loop (radiator, reservoir, pump).
*   Units designed to be modular and hot-swappable.
*   Integration with rack-level power distribution units (PDUs) for synchronized power and cooling control.