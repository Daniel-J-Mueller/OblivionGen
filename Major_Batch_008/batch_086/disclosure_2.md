# 9190870

**Dynamic Phase-Shifted Redundancy for High-Density Racks**

**Concept:** Implement a system allowing for *dynamic* phase shifting of redundant power feeds within a high-density rack. Instead of simply mirroring power distribution, this system actively adjusts the phase angle of the secondary (redundant) feed relative to the primary.

**Rationale:** Traditional redundant power systems rely on parallel connections with identical phase. This works, but creates circulating currents and inefficiencies. By slightly shifting the phase of the redundant feed, we can *reduce* circulating currents, improve overall efficiency, and potentially allow for *higher* total power delivery within the same infrastructure. More critically, a controlled phase shift can act as an early indicator of potential failures *before* a complete outage – a deviation from the programmed phase shift signals an issue.

**System Specifications:**

1.  **Phase-Shift Modules (PSM):**  Each rack will incorporate PSMs for both primary and redundant feeds. These modules contain solid-state phase shifters (e.g., using controllable reactance) capable of adjusting phase angle from -30° to +30° relative to the primary feed. Resolution: 0.1°.

2.  **Rack Power Controller (RPC):** A central controller within the rack monitors current draw, voltage, and phase angle on both feeds. The RPC implements a closed-loop control algorithm.

3.  **Control Algorithm (Pseudocode):**

```
// Variables
primary_current = current reading from primary feed
redundant_current = current reading from redundant feed
primary_voltage = voltage reading from primary feed
redundant_voltage = voltage reading from redundant feed
phase_shift = current phase shift setting
optimal_shift = calculated optimal phase shift
error = difference between current and optimal phase shift

// Initialization
Set phase_shift = 0 degrees
Set sampling_rate = 100Hz
Set max_phase_shift_change_rate = 0.5 degrees/second // Prevents rapid oscillations

// Main Loop
While (system_running) {
    Read primary_current, redundant_current, primary_voltage, redundant_voltage
    Calculate total_current = primary_current + redundant_current
    Calculate optimal_shift = Function(total_current, primary_voltage, redundant_voltage)  // Algorithm to minimize circulating currents based on load.
    error = optimal_shift - phase_shift
    If (Abs(error) > 0.1 degrees) {
        If (error > 0) {
            phase_shift = Min(phase_shift + max_phase_shift_change_rate, 30 degrees) //Limit Phase Shift To Max
        } Else {
            phase_shift = Max(phase_shift - max_phase_shift_change_rate, -30 degrees) //Limit Phase Shift To Min
        }
    }
    Set Phase Shifter To Phase Shift Angle
    Delay(10ms)
}
```

4.  **Monitoring and Alerting:** The RPC will continuously monitor the phase angle difference between the feeds. A deviation beyond a pre-defined threshold will trigger an alert.

5.  **Communication:** The RPC will communicate rack-level power data and status to a central data center infrastructure management (DCIM) system.

6.  **Hardware Components:**
    *   Solid-state phase shifters (IGBT or similar)
    *   High-precision current and voltage sensors
    *   Microcontroller or FPGA for control logic implementation
    *   Communication interface (Ethernet, Modbus, etc.)

7. **Integration with Existing Systems**: Existing UPS and PDU infrastructure will function as usual, providing the raw power. The system is an 'add-on' for enhanced rack-level control and monitoring.

8. **Safety Features**: Overcurrent protection, overvoltage protection, and thermal monitoring will be integrated into the system. Hardware interlocks will prevent excessive phase shifts.