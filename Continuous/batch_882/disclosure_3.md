# 11221154

## Adaptive Thermal Chimney with Integrated Biofiltration

**Concept:** Expand the passive exhaust concept to not only remove warm air but actively regulate building temperature and improve air quality via a biofiltration system integrated within a dynamically adjustable chimney structure.

**Specifications:**

*   **Module Construction:** Each module comprises a multi-layered shell.
    *   Outer Layer: UV-resistant, weather-sealed polymer.
    *   Mid Layer: Phase change material (PCM) – paraffin wax or similar – to absorb/release heat, providing thermal buffering. PCM density adjustable based on climate zone.
    *   Inner Layer: Network of vertically oriented capillary tubes embedded within a growing medium (e.g., coconut coir, perlite).
*   **Biofiltration System:**
    *   Plant Selection:  Hydroponically grown plants selected for high transpiration rates and air purification capabilities (e.g., Peace Lily, Snake Plant, Spider Plant).  Automated plant rotation/replacement system for optimal performance.
    *   Nutrient Delivery: Automated nutrient delivery system (pH and EC monitoring) integrated into the capillary tube network.  Recycled greywater usage where permissible.
    *   Airflow Management: Air drawn *through* the plant root systems via capillary action and a low-power fan array (solar-powered option).
*   **Dynamic Adjustment:**
    *   Variable Geometry: Each module features telescoping sections, controlled by linear actuators. This allows the chimney height (and thus draft) to be adjusted based on temperature differentials and wind conditions.  Automated adjustments based on internal/external sensors.
    *   Louvre System: Adjustable louvres at the base of each module regulate airflow intake.  Orientation responsive to prevailing wind direction for maximized draft.
    *   Thermal Dampers: Internal dampers control airflow through the PCM layer, optimizing heat absorption/release based on time of day/year.
*   **Sensor Suite:** Integrated sensor suite monitors:
    *   Internal/External Temperature
    *   Humidity
    *   Air Quality (VOCs, CO2, particulate matter)
    *   Wind Speed & Direction
    *   Plant Health (moisture levels, nutrient uptake)
*   **Control System:** Centralized control system (IoT-enabled) manages all module functions.  Remote monitoring & control via mobile app.  Predictive algorithms optimize system performance based on weather forecasts.
*   **Module Array:** Modules arranged in a grid pattern on the roof.  Array size scalable based on building size and ventilation requirements.  Inter-module communication for coordinated airflow management.

**Pseudocode (Control System - Dynamic Adjustment):**

```
// Main Loop
while (true) {
  // Read Sensor Data
  temp_internal = read_temperature(internal_sensor);
  temp_external = read_temperature(external_sensor);
  humidity_internal = read_humidity(internal_sensor);
  wind_speed = read_wind_speed(wind_sensor);
  air_quality = read_air_quality(air_quality_sensor);

  // Calculate Temperature Differential
  temp_diff = temp_internal - temp_external;

  // Adjust Chimney Height
  if (temp_diff > threshold_high AND wind_speed < threshold_wind_low) {
    extend_chimney(module_id, amount = height_increase);
  } else if (temp_diff < threshold_low AND wind_speed > threshold_wind_high) {
    retract_chimney(module_id, amount = height_decrease);
  }

  // Adjust Louvres
  if (wind_direction == "north") {
    set_louvre_angle(module_id, angle = 0); // Open towards north
  } else if (wind_direction == "south") {
    set_louvre_angle(module_id, angle = 180); // Open towards south
  }

  //Adjust Thermal Dampers
  if (time_of_day == "day") {
    close_dampers(module_id); //minimize PCM heat absorbtion
  } else if (time_of_day == "night") {
    open_dampers(module_id); //maximize PCM heat absorbtion
  }

  //Air Quality Control
  if (air_quality < acceptable_level){
    increase_fan_speed(module_id);
  } else {
    decrease_fan_speed(module_id);
  }

  sleep(update_interval);
}
```