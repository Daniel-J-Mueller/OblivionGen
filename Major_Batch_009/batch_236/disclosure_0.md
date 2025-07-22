# 9332670

## Modular, Active Floor System for Data Centers

**Concept:** A data center floor comprised of individually addressable, actively-damped modules, integrating power and cooling alongside leveling capabilities, extending beyond static pad compression.

**Specifications:**

*   **Module Dimensions:** 60cm x 60cm x 20cm (scalable).  Modules interlock to form a continuous floor surface.
*   **Frame:** High-strength aluminum alloy, designed to distribute load and house internal components. Internal bracing every 15cm.
*   **Leveling System:** Each module contains four (minimum) independent, digitally-controlled pneumatic actuators at each corner. Actuators interface with a central control system. Range of motion: +/- 5cm. Resolution: 0.1mm.  Actuators incorporate absolute position encoders for closed-loop control.
*   **Damping System:**  Magneto-rheological (MR) fluid dampers integrated within the module frame.  Fluid viscosity controlled by an electromagnetic field, adjustable in real-time to counteract vibrations and shock.  Damping force configurable per module via the central control system. Frequency response targeted 5-50Hz.
*   **Power Distribution:** Each module incorporates a solid-state power distribution unit (PDU) capable of delivering up to 20kW. Power input: 480V 3-phase. Outputs: Configurable mix of 120V/240V AC and DC.  Each PDU incorporates surge protection and current monitoring.
*   **Cooling Integration:**  Each module features integrated liquid cooling channels.  Channels connect to a central chilled water loop.  Variable flow rate control per module based on heat load. Microchannel heat exchangers positioned directly under high-density rack locations.
*   **Sensor Suite:** Each module integrates:
    *   Inertial Measurement Unit (IMU) – Detects vibrations, tilts, and accelerations.
    *   Temperature Sensors – Monitors module and rack inlet temperatures.
    *   Strain Gauges – Measures load distribution within the module.
    *   Acoustic Sensors – Monitors noise levels.
*   **Communication:** Modules communicate via a wired Ethernet network with Power over Ethernet (PoE). Protocol:  A custom data protocol for real-time control and sensor data transmission.
*   **Control System:** A centralized server running a custom control software application. Features:
    *   Real-time floor map visualization.
    *   Automated floor leveling based on sensor data.
    *   Dynamic vibration damping.
    *   Predictive maintenance alerts.
    *   Integration with data center infrastructure management (DCIM) systems.
*   **Power Supply:** Redundant power supplies for critical components.
*    **Materials:**  Non-conductive, fire-resistant materials used for all internal components and module surfaces.

**Operation:**

1.  The system continuously monitors floor vibration, tilt, and temperature using the integrated sensor suite.
2.  The control system uses this data to dynamically adjust the pneumatic actuators, leveling the floor and compensating for variations in weight distribution.
3.  The MR fluid dampers actively counteract vibrations, minimizing the risk of damage to sensitive equipment.
4.  The integrated power and cooling systems provide efficient and reliable power and cooling to the racks.
5.  The control system provides a comprehensive view of the floor's performance and alerts administrators to potential issues.