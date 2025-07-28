# 9768772

## Adaptive Polarity Wireless Power Transfer System

**Concept:** A wireless power transfer (WPT) system that utilizes the polarity-adaptive bridge circuit as a *receiver* component, but extends its functionality to actively *shape* the received power profile for optimal device charging, even with fluctuating input power or mismatched orientations.

**Specs:**

*   **Receiver Core:** Employs the four MOSFET bridge circuit detailed in the patent. This forms the base for receiving the WPT signal.
*   **Power Profiling Module:**  Integrated microcontroller (STM32 or equivalent) monitors the voltage & current *after* the bridge circuit. It also interfaces with a device identification module.
*   **Device Identification:**  Near-field communication (NFC) or Bluetooth Low Energy (BLE) chip for identifying the device being charged. This enables lookup of optimal charging profiles.
*   **Dynamic Resistance Adjustment:** The microcontroller controls variable resistors (digital potentiometers) connected to the outputs of the bridge circuit.  These fine-tune the output voltage and current.
*   **Orientation Compensation:**  Multiple small receiving coils arranged in an array, connected to different sections of the bridge circuit, and dynamically weighted by the microcontroller. This maximizes power capture regardless of relative orientation.
*   **Power Shaping Algorithms:** The microcontroller implements algorithms to:
    *   **Constant Current (CC) Mode:** Delivers a constant current to the device, ideal for batteries.
    *   **Constant Voltage (CV) Mode:**  Maintains a constant voltage, suitable for devices with voltage regulators.
    *   **Pulse Charging:**  Applies short bursts of power to improve charging efficiency and reduce heat.
    *   **Trickle Charging:**  Maintains a full charge without overcharging.
*   **Wireless Communication (Optional):**  A low-power wireless module (e.g., Zigbee) to transmit charging status and receive commands from a central control system.
*   **Power Monitoring & Safety:** Built-in over-voltage, over-current, and thermal protection circuits.

**Pseudocode (Charging Cycle):**

```
INIT:
  NFC/BLE Scan for Device ID
  Load Device Charging Profile (Voltage/Current Limits, Charging Mode)

LOOP:
  Monitor Input Power (WPT Signal)
  Monitor Output Voltage & Current
  Adjust Variable Resistors to Maintain Target Voltage/Current (PID control)
  Implement Charging Mode (CC, CV, Pulse, Trickle)
  If Input Power Drops Below Threshold:
    Reduce Output Power Accordingly
    Enter Low-Power Mode
  If Device is Fully Charged:
    Enter Trickle Charge Mode
    Or Stop Charging
  If Error Condition Detected (Over-Voltage, Over-Current, Thermal):
    Disable Output & Trigger Alarm
```

**Novelty:**  This system isn't just adapting polarity. It's actively shaping the *delivered* power profile based on the device being charged and the available power, creating a more efficient and versatile WPT receiver.  The dynamic resistance adjustment and orientation compensation are key differentiators.