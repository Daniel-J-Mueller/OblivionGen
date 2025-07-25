# 9346606

## Self-Orienting Reveal Package

**Concept:** A package that, upon initial opening, automatically orients the revealed item towards the user, regardless of the package's initial resting orientation. This builds on the 'reveal' aspect of the original patent, adding a layer of user-centric presentation.

**Specs:**

*   **Housing Material:** Lightweight, rigid plastic or cardboard composite. Must be opaque to conceal contents.
*   **Internal Gimbal System:** A two-axis (pitch/roll) gimbal system housed *within* the package. The gimbal cradles the product securely. A small, low-power DC motor with a gear reduction drives each axis.
*   **Orientation Sensor:** A MEMS inertial measurement unit (IMU – accelerometer + gyroscope) detects the package’s initial resting orientation (using gravity as a reference).
*   **Microcontroller:** A small microcontroller (e.g., ESP32 or similar) receives data from the IMU, calculates the necessary gimbal adjustments, and controls the DC motors.
*   **Power Source:** A small, flat coin cell battery or integrated wireless charging receiver.
*   **Activation Mechanism:** Opening the package initiates the process. A simple mechanical switch triggered by the drawbridge/panel movement activates the microcontroller.
*   **Drawbridge/Panel Integration:** The drawbridge/panel *does not* directly move the product. Instead, it triggers the orientation sequence. The product remains stationary on the gimbal until fully revealed.
*   **Reveal Sequence:**
    1.  User initiates opening the drawbridge/panel.
    2.  Mechanical switch activates the microcontroller.
    3.  Microcontroller reads the IMU to determine the initial package orientation.
    4.  Microcontroller calculates the necessary gimbal adjustments to bring the product to a pre-defined ‘upright’ or ‘facing-user’ orientation.
    5.  DC motors rotate the gimbal to achieve the desired orientation.
    6.  Drawbridge/panel continues to open, fully revealing the oriented product.
*   **Software:** Firmware for the microcontroller. Key features:
    *   IMU data acquisition and processing.
    *   Gimbal control algorithms.
    *   Power management.
    *   Calibration routine.
*   **Safety Features:**
    *   Current limiting on motor drivers.
    *   Mechanical stops to prevent over-rotation.
    *   Emergency stop function (e.g., rapid drawbridge opening disables motors).

**Pseudocode (Microcontroller):**

```
// Initialization
Initialize IMU
Initialize Motors
Calibrate IMU

// Main Loop
While (Package Not Fully Opened)
    Read IMU Data
    Calculate Target Gimbal Angles (based on IMU data)
    Control Motors to Rotate Gimbal to Target Angles
    If (Package Fully Opened)
        Disable Motors
        Exit Loop
```

**Materials:**

*   Rigid plastic or cardboard
*   Electronic components (IMU, microcontroller, motors, battery)
*   Gimbal bearings/structure
*   Wiring/connectors
*   Adhesive

**Manufacturing Notes:**

*   Modular design for easy assembly and component replacement.
*   Integration of electronics into the package structure.
*   Quality control testing of gimbal alignment and motor functionality.