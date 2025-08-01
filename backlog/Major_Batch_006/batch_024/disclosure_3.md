# 9768772

## Adaptive Polarity Wireless Power Transfer

**Concept:** Extend the polarity-adaptive bridge circuit to enable wireless power transfer with orientation-agnostic charging.

**Specifications:**

*   **Core Component:** Polarity-adaptive bridge circuit (based on the provided patent’s FET configuration – N & P type MOSFETs) integrated into both a transmitting and receiving coil system.
*   **Coil Geometry:** Utilize a multi-coil array (e.g., a 3x3 grid) on both transmitter and receiver. Each coil element in the array is connected to a dedicated polarity-adaptive bridge circuit.
*   **Communication Protocol:** Implement a low-power Bluetooth communication link between the transmitter and receiver. The receiver reports the optimal coil selection and corresponding bridge configuration (FET activation) to the transmitter.
*   **Transmitter Operation:**
    *   The transmitter initiates power delivery with all coil elements active.
    *   Receives data from the receiver indicating the coil(s) with the strongest coupling and optimal bridge configuration.
    *   Dynamically adjusts power distribution to the selected coil(s) while maintaining the prescribed bridge configuration (polarity adaptation).
    *   Continuously monitors coupling strength and re-optimizes power delivery based on receiver feedback.
*   **Receiver Operation:**
    *   The receiver scans all incoming magnetic fields from the transmitter’s coil array.
    *   Identifies the coil(s) providing the strongest magnetic coupling.
    *   Determines the required bridge configuration (FET activation) to rectify the incoming AC signal and achieve the desired DC output voltage. This decision is based on the detected polarity of the induced current.
    *   Transmits the coil selection and bridge configuration data to the transmitter via Bluetooth.
*   **Power Management:** Implement a fast-response DC-DC converter on the receiver side to regulate the output voltage and current.
*   **Safety Features:** Integrate over-voltage, over-current, and temperature protection circuitry on both transmitter and receiver.
*   **Pseudocode (Receiver Side – Coil Selection & Bridge Configuration):**

```
// Initialize variables
coil_array_size = 9
coupling_strengths[coil_array_size]
bridge_configuration[coil_array_size]
max_coupling = 0
best_coil = 0

// Scan for coupling strength on each coil
for i = 0 to coil_array_size - 1
    coupling_strengths[i] = measure_coupling_strength(coil[i])

    // Determine bridge configuration based on polarity
    polarity = detect_induced_current_polarity(coil[i])
    if polarity == POSITIVE:
        bridge_configuration[i] = "FET1_ON, FET2_OFF, FET3_ON, FET4_OFF"
    else:
        bridge_configuration[i] = "FET1_OFF, FET2_ON, FET3_OFF, FET4_ON"

    // Find coil with max coupling
    if coupling_strengths[i] > max_coupling:
        max_coupling = coupling_strengths[i]
        best_coil = i

// Transmit data to transmitter
transmit_data(transmitter_ID, best_coil, bridge_configuration[best_coil])
```

**Potential Applications:**

*   Wireless charging pads with true 360-degree orientation freedom.
*   Implantable medical devices with wireless power harvesting.
*   Robotics and automation applications requiring dynamic wireless power transfer.
*   Charging of mobile devices, laptops, and other portable electronics without precise alignment.