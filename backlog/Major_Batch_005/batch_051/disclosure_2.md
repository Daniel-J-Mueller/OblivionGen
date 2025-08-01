# 8612786

## Adaptive Clock Domain Partitioning

**Concept:** Dynamically reconfigure clock domains within the device *during* deep idle, beyond simply switching to an external clock. This enables granular power savings by isolating and shutting down inactive functional units at the clock level, minimizing leakage current.

**Specs:**

*   **Hardware:**
    *   Clock Domain Controller (CDC): A dedicated hardware block with programmable clock gating and switching capabilities.
    *   Power Domain Controller (PDC): Interfaced with the CDC, managing power supply to individual functional blocks.
    *   Clock Tree Synthesis:  Clock tree must be synthesizable to allow for runtime reconfiguration.  Support for multiple clock sources (MPLL, external low-frequency oscillator, and potentially RC oscillators for ultra-low power).
    *   Functional Block Identification: Each functional block (CPU core, GPU, memory controller, peripherals) must have unique identification for software control.
    *   Low-Drop Voltage Regulators:  For precise power control to individual blocks.
*   **Software/Firmware:**
    *   Idle State Analyzer: Analyzes system activity to identify inactive functional blocks.  Uses a combination of hardware performance counters and software profiling.
    *   Clock Domain Partitioning Algorithm: Algorithm dynamically reconfigures clock domains based on Idle State Analyzer output.  Prioritizes functional blocks with the highest leakage current for power saving. Algorithm must also consider transition costs/latencies for switching clock domains.
    *   Clock Gating Granularity:  Support for clock gating at the individual functional block level.
    *   Power Management Driver:  Interface between operating system and Clock Domain Partitioning Algorithm.
    *   Runtime Clock Tree Modification:  Software-controlled clock tree synthesis/modification engine.

**Pseudocode (Clock Domain Partitioning Algorithm):**

```
// Initialization
active_blocks = {CPU, Memory, Basic Peripherals}; // Initial set of active blocks
clock_tree = generate_initial_clock_tree(active_blocks);

// Main Loop (executed during Deep Idle)
while (system_is_idle()) {
  inactive_blocks = identify_inactive_blocks(); // Utilize hardware performance counters + software profiling

  if (inactive_blocks.size() > 0) {
    for each block in inactive_blocks {
      // Isolate the block (stop clocking it)
      isolate_block(block);

      // Shut off power to the block (optional)
      shutdown_power(block);

      // Update Clock Tree
      update_clock_tree(block);

      remove_block_from_active_list(block);
    }
  }

  // Wake-up event received?
  if (wake_up_event_received()) {
    restore_clock_domains();
    restore_power_domains();
    break;
  }
}
```

**Innovation:**  Goes beyond simple clock switching to a *dynamic* partitioning of clock domains, minimizing overall power consumption by completely isolating inactive blocks.  The dynamic nature, driven by runtime analysis, allows for adaptation to varying workloads and usage patterns.