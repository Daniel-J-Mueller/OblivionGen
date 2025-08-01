# 9681588

**Modular Sub-Floor Liquid Distribution & Phase Change Cooling**

**Concept:** Expand on the liquid distribution network, but shift from solely condensation-based cooling to incorporate modular, replaceable phase change material (PCM) cartridges integrated *within* a redesigned sub-floor system. This creates a more dynamic and efficient cooling solution, reducing reliance on ambient humidity and enabling precise temperature control.

**Specifications:**

*   **Sub-Floor Grid:** Replace traditional raised flooring with a modular grid system composed of interlocking, thermally conductive tiles. Each tile contains channels for liquid distribution and cartridge placement. Tile material: Aluminum alloy with high thermal conductivity. Dimensions: 60cm x 60cm x 15cm.
*   **PCM Cartridges:** Sealed cartridges containing a PCM with a melting point optimized for data center operating temperatures (e.g., 18-22°C). Cartridge Material: High-density polyethylene. Dimensions: 30cm x 30cm x 5cm. Each tile accommodates 4 cartridges.
*   **Liquid Distribution Network:** A closed-loop system circulating a dielectric coolant (e.g., deionized water with corrosion inhibitors). Coolant is chilled by a central chiller unit. Liquid pathways are integrated within the sub-floor tiles.
*   **Microchannel Heat Exchangers:** Embedded within each tile, directly above the PCM cartridges. Coolant flows through the microchannels, transferring heat to the PCM.
*   **Airflow Management:** Perforated tiles direct airflow upwards through the PCM cartridges and microchannel heat exchangers before entering the cold aisle. Adjustable airflow dampers on each tile enable fine-tuning of cooling to specific rack locations.
*   **Monitoring & Control:** Each tile is equipped with temperature sensors and flow meters. A central control system monitors tile performance and adjusts coolant flow and airflow to maintain optimal temperatures. Remote access and automated adjustment capabilities are standard.
*   **Cartridge Replacement System:** Automated robotic system for detecting depleted PCM cartridges and replacing them with fully charged cartridges. Minimal downtime and reduced maintenance requirements.
*   **Redundancy:** Multiple redundant coolant pumps and chillers. Cartridge replacement system includes spare cartridges.
*   **Power Supply:** 48V DC power distributed through the sub-floor grid to power sensors, actuators, and the cartridge replacement system.

**Pseudocode (Control System):**

```
// Main Loop
while (true) {
  // Read Temperature Data from all Tiles
  tileData = readTileData();

  // Calculate Average Temperature
  avgTemp = calculateAverageTemperature(tileData);

  // Compare to Setpoint
  if (avgTemp > setpoint) {
    // Increase Coolant Flow
    increaseCoolantFlow();
    //Adjust airflow
    adjustAirflow(tileData);
  } else if (avgTemp < setpoint) {
    // Decrease Coolant Flow
    decreaseCoolantFlow();
  }
  // Check for Depleted PCM Cartridges
  depletedCartridges = identifyDepletedCartridges(tileData);
  // Initiate Cartridge Replacement Process
  replaceCartridges(depletedCartridges);
}
```

**Innovation:** This design moves beyond passive condensation cooling to incorporate active phase change materials and a dynamic, modular sub-floor system. The integration of PCM allows for thermal buffering and reduces the load on the central chiller unit. The modular design simplifies maintenance and enables targeted cooling to specific rack locations. The automated monitoring and control system optimizes cooling performance and minimizes energy consumption.