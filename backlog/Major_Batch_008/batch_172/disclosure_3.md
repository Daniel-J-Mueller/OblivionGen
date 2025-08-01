# D966194

## Adaptive Polarity Plug Adapter

**Concept:** A plug adapter capable of dynamically reconfiguring its internal wiring to match the polarity of the receiving outlet, mitigating damage from reversed polarity and offering wider compatibility across diverse electrical systems.

**Specs:**

*   **Form Factor:** Retains a similar overall size and shape to standard plug adapters, but incorporates a slightly larger housing to accommodate internal components. Housing material: Flame-retardant polycarbonate.

*   **Core Component:** A micro-relay array (minimum 4 relays) controlled by a microcontroller. Relays are rated for at least 15A/125VAC.

*   **Polarity Detection:** Two recessed probes extending from the plug prongs. These probes make initial contact with the outlet’s internal contacts *before* the main prongs fully seat. Resistance measurement between the probes determines polarity.  Accuracy: +/- 5% resistance differential.

*   **Microcontroller:** ESP32-S3 or similar, chosen for integrated Wi-Fi/Bluetooth capabilities.  Programming language: MicroPython.  Firmware includes a fault-detection algorithm to disable the adapter if a critical error is detected.

*   **Relay Configuration:**
    *   Relay 1: Connects Line (Hot) to corresponding outlet contact.
    *   Relay 2: Connects Neutral to corresponding outlet contact.
    *   Relay 3 & 4: Act as switching relays to reverse Line and Neutral if reversed polarity is detected.

*   **Operational Sequence:**
    1.  Plug is inserted into outlet. Probes establish contact.
    2.  Microcontroller measures resistance between the probes.
    3.  If resistance indicates standard polarity: Relays 1 & 2 activate, bypassing Relays 3 & 4.
    4.  If resistance indicates reversed polarity: Relays 3 & 4 activate, swapping Line and Neutral connections.
    5.  Main plug prongs fully seat, completing the circuit.
    6.  Optional:  Adapter transmits polarity information to a connected mobile app via Bluetooth.

*   **Safety Features:**
    *   Thermal fuse: Trips at 85°C.
    *   Overcurrent protection: Integrated current limiting circuit.
    *   Housing grounded to reduce shock hazard.
    *   LED indicator:  Green = Normal Polarity, Red = Reversed Polarity (Corrected).

*   **Power Requirements:**  Adapter draws minimal power from the outlet (less than 0.5W) to operate the microcontroller and relays.

**Pseudocode (Polarity Detection & Relay Control):**

```python
def detect_polarity():
  resistance = measure_resistance(probe1, probe2)
  if resistance > threshold_standard:
    return "reversed"
  else:
    return "standard"

def control_relays(polarity):
  if polarity == "standard":
    activate_relay(relay1)
    activate_relay(relay2)
    deactivate_relay(relay3)
    deactivate_relay(relay4)
  elif polarity == "reversed":
    deactivate_relay(relay1)
    deactivate_relay(relay2)
    activate_relay(relay3)
    activate_relay(relay4)

def main():
  polarity = detect_polarity()
  control_relays(polarity)
  # Optional: Transmit polarity data via Bluetooth

if __name__ == "__main__":
  main()
```