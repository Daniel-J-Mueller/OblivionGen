# 10799734

## Sub-Floor Thermal Regulation & Fire Suppression – Integrated Phase Change Material (PCM) System

**System Overview:**

This system builds upon the concept of sub-floor fire suppression but integrates thermal management using Phase Change Materials (PCMs) to proactively address heat buildup *before* it becomes a fire risk and enhance fire suppression effectiveness. The goal is to create a self-regulating, thermally stable sub-floor environment.

**Components:**

1.  **PCM Encapsulated Tiles:** Standard raised floor tiles are replaced with tiles containing sealed PCM capsules. The PCM is selected based on a melting point that coincides with typical operating temperatures of sub-floor electrical/cooling equipment (e.g., 30-40°C). Capsule material must be fire-resistant and chemically inert to prevent interaction with suppression agents.  Capsule dimensions: 5cm diameter spheres, spaced 10cm apart within the tile matrix. Tile construction: High-density polymer composite with integrated capsule mounting structure.

2.  **Thermal Monitoring Network:** Each tile is equipped with a micro-thermistor connected to a central monitoring system. The system provides real-time temperature mapping of the sub-floor space and logs data for predictive maintenance and anomaly detection. Thermistor accuracy: ±0.5°C. Sampling rate: 1 Hz.

3.  **Active Cooling/Heating Loops (Optional):** For high-density deployments, small-diameter, fluid-filled loops are embedded within the tile structure. These loops connect to a central cooling/heating unit and provide supplemental temperature regulation as needed, overriding or augmenting the PCM’s natural behavior. Fluid: Dielectric coolant (e.g., 3M Novec). Loop diameter: 6mm.

4.  **Localized Fire Suppression Nozzles:** The fire suppression system is maintained, but nozzles are repositioned to maximize coverage of areas with high thermal signatures detected by the monitoring network. Nozzle type: Micro-mist atomizers for efficient agent distribution.

5.  **Pressure-Sensitive Leak Detection:** Each PCM capsule is equipped with a micro-pressure sensor. A sudden pressure drop indicates capsule breach. The system triggers an alarm and isolates the affected tile.

**Operational Logic (Pseudocode):**

```
// Central Control System

loop {
  for each tile in tile_array {
    temperature = read_temperature(tile)
    pressure = read_pressure(tile)

    if (pressure < threshold) {
      log_event("Capsule breach detected on tile " + tile.id)
      isolate_tile(tile)
      activate_local_suppression(tile) //Preemptive, localized suppression
    }

    if (temperature > high_threshold) {
      log_event("High temperature detected on tile " + tile.id)
      if (active_cooling_enabled) {
        activate_cooling_loop(tile)
      }
      activate_local_suppression(tile) // Preemptive action
    }
  }
}
```

**Tile Specifications:**

*   Dimensions: 60cm x 60cm x 10cm
*   Weight (fully loaded): 15kg
*   Material: High-density polymer composite with integrated PCM capsule mounting structure.
*   Capsule count per tile: 100
*   Capsule volume: 25 ml
*   Capsule Material: Fire-resistant, chemically inert polymer

**PCM Selection:**

*   Material: Paraffin wax blend with melting point of 35°C.
*   Latent heat of fusion: 200 kJ/kg
*   Density: 900 kg/m3

**Maintenance:**

*   Automated system health checks.
*   Scheduled inspection of PCM capsules for damage.
*   Data analysis of thermal patterns to identify potential issues.