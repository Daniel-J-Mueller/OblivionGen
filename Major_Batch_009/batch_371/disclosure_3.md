# D842381

## Modular Kinetic Illuminated Signage

**Concept:** A sign system composed of individually addressable, mechanically actuated illuminated panels. Instead of static illumination or simple animation via light, the *physical* surface of the sign dynamically reshapes to create complex 3D visuals and moving text.

**Hardware Specifications:**

*   **Panel Modules:** 10cm x 10cm x 5cm cubes. Each cube contains:
    *   Micro-actuators (shape memory alloy or miniature linear actuators) - minimum 8 per panel, strategically placed to allow for controlled deformation.
    *   RGB LED array (addressable, high density) embedded within a translucent, durable polymer shell.
    *   Microcontroller (ESP32 or similar) for individual panel control and communication.
    *   Power supply - integrated miniature solid-state battery + wireless charging receiver.
    *   Magnetic interlocking system – allows panels to snap together securely on all six faces.

*   **Interconnect System:**  Panels connect via strong neodymium magnets embedded within the interlocking edges.  Magnet configuration allows for both physical stability *and* data/power transfer via inductive coupling between adjacent panels.

*   **Control System:**
    *   Central Controller (Raspberry Pi or similar) – communicates with panels via a mesh network (WiFi or custom RF protocol).
    *   Software –  A visual programming interface allows users to create kinetic animations.  The software translates animations into actuator control signals for each panel.
    *   Real-time physics engine – simulates panel deformations to prevent collisions and ensure smooth movements.

**Software/Algorithm Specifications:**

*   **Animation Pipeline:**
    1.  User creates 3D animation in software.
    2.  Software slices animation into discrete frames.
    3.  For each frame:
        *   The system calculates the necessary deformation for each panel to approximate the desired 3D shape.  This involves solving an inverse kinematics problem.
        *   The system generates actuator control signals for each panel, specifying the desired position of each actuator.
        *   The control signals are transmitted to the panels via the mesh network.
*   **Actuator Control:**
    *   Proportional-Integral-Derivative (PID) control algorithm used to precisely control actuator position.
    *   Force sensors integrated into actuators provide feedback for closed-loop control.
    *   Collision detection algorithm prevents panels from colliding during movement.
*   **Mesh Networking:**
    *   Panels communicate with each other to form a self-organizing mesh network.
    *   The network automatically routes data and power between panels.
    *   The network is resilient to panel failures.

**Materials:**

*   **Panel Shell:** High-impact translucent polycarbonate or acrylic.
*   **Actuators:** Shape memory alloy (SMA) or miniature linear actuators.
*   **Magnets:** Neodymium magnets with protective coating.
*   **Electronics:** Standard miniature electronic components.