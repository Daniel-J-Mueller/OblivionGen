# D990022

## Modular Bioluminescent Wall Light System

**Concept:** A wall light system utilizing modular, geometrically shaped panels capable of supporting bioluminescent organisms (engineered bacteria or algae) within a transparent, nutrient-rich gel matrix. The panels connect magnetically, allowing for customizable wall displays that *grow* light rather than *emit* it.

**Panel Specs:**

*   **Shape:** Primarily equilateral triangles, with options for squares and hexagons for design variation.
*   **Dimensions:** 15cm side length (triangle), 15cm x 15cm (square/hexagon).
*   **Material:** Transparent acrylic or polycarbonate shell.  Must be UV-permeable to facilitate algae/bacteria growth.
*   **Internal Structure:** Honeycomb matrix within the panel, creating small individual ‘cells’ for the bioluminescent organisms.  Each cell is approx. 1cm cubed.
*   **Nutrient Delivery:** Microfluidic channels integrated into the honeycomb structure, delivering a nutrient-rich gel (agar-based) to each cell.  Channels connect to a central reservoir/pump system (described below).
*   **Magnetic Connectors:**  Strong neodymium magnets embedded in each edge of the panel. Polarities are consistent for ease of connection, though rotational freedom is permitted.
*   **Diffusion Layer:**  A thin, frosted layer on the exterior of the panel to diffuse the bioluminescence, creating a softer glow.
*   **UV Filtering:** Selective UV filtering on the exterior to optimize light output and protect organisms.

**Central Control Unit Specs:**

*   **Reservoir:** 1L capacity, for nutrient gel storage.
*   **Pump System:** Peristaltic pump capable of delivering precise amounts of nutrient gel to each panel via the microfluidic channels.  Flow rate adjustable per panel.
*   **Sensors:**  pH, temperature, and luminescence sensors within the reservoir to monitor organism health and adjust nutrient delivery accordingly.
*   **Power:** Standard AC power input.
*   **Control Interface:** WiFi-enabled microcontroller (ESP32) allowing for remote control and monitoring via a mobile app.
*   **Software:**  App allows for:
    *   Customizable panel brightness (via nutrient delivery rate).
    *   Panel grouping and synchronized light patterns.
    *   Real-time monitoring of organism health metrics.
    *   Automated nutrient replenishment scheduling.

**Organism Considerations:**

*   *Aliivibrio fischeri* (bacteria) or engineered bioluminescent algae strains are preferred.
*   Organisms are encapsulated within the nutrient gel matrix to prevent contamination and maintain cell viability.
*   Gel composition must be optimized for organism growth and bioluminescence.
*   Closed-loop system to recycle waste products and maintain a stable internal environment.

**Assembly:**

1.  Nutrient gel is prepared and inoculated with the bioluminescent organisms.
2.  Gel is pumped into the honeycomb matrix of each panel via the microfluidic channels.
3.  Panels are magnetically connected to form a desired wall display.
4.  Control unit is connected to the nutrient reservoir and power supply.
5.  Control software is used to configure and monitor the system.