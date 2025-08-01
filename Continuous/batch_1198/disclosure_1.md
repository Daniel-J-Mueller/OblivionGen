# 9664577

## Variable Stiffness Force Sensor Array

**Concept:** A force sensor array utilizing variable stiffness elements created via microfluidic channels embedded within the FSR substrate. This allows for localized control of sensor sensitivity and dynamic adaptation to varying force ranges.

**Specifications:**

*   **Substrate:** Polyimide base (as per patent) with integrated microfluidic channel network. Channel dimensions: 50-200µm width, 20-100µm height. Channel material: PDMS or similar flexible polymer.
*   **FSR Material:** Thermoset ink (carbon black/silver particles) formulated for compatibility with microfluidic channel material. Ink deposited in a patterned array over microfluidic channels.
*   **Actuation Fluid:** Electrorheological (ER) or Magnetorheological (MR) fluid. Fluid chosen for rapid viscosity change with low voltage/magnetic field application.
*   **Microfluidic Control:** Each channel controlled by integrated micro-pumps/valves (MEMS based) for precise fluid delivery and removal.
*   **Electrode Integration:** Micro-electrodes integrated into channel walls for ER fluid control, or micro-coils for MR fluid control.
*   **Sensor Architecture:** Array of individual sensing elements. Each element comprises a section of the FSR material bridging the microfluidic channel.
*   **Sensing Mechanism:**  Force applied deforms the FSR material. The deformation changes the resistance of the FSR. By altering the viscosity of the fluid within the channel, the stiffness of the FSR element can be controlled. This effectively tunes the sensitivity of each sensor in the array.
*   **Control System:** Algorithm to dynamically adjust fluid viscosity (and therefore sensor stiffness) based on applied force and desired sensitivity.
*   **Calibration:** Individual sensor elements calibrated to establish baseline resistance and force-response curve.

**Pseudocode (Control Algorithm):**

```
// Initialize: Baseline Resistance (R0) for each sensor element
//              Force Thresholds (High/Low)
//              Target Sensitivity Levels

loop:
    read resistance (R) from each sensor element
    calculate force (F) = function(R) // based on calibrated curve

    if F > HighThreshold:
        decrease fluid viscosity in corresponding channel
        increase sensitivity
    elif F < LowThreshold:
        increase fluid viscosity in corresponding channel
        decrease sensitivity
    else:
        maintain current viscosity

    adjust micro-pump/valve to achieve target viscosity
```

**Potential Applications:**

*   High-resolution tactile sensors for robotics
*   Adaptive prosthetic limbs
*   Biometric authentication (pressure mapping)
*   Medical imaging (pressure distribution)
*   Gaming controllers (force feedback)