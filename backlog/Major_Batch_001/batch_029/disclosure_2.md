# 10022937

## Dynamic Pressure Mapping for Flexible Displays

**Concept:** Utilizing microfluidic channels *within* the flexible substrates themselves to create localized pressure adjustments during lamination, enabling precise control over adhesion and minimizing stress concentrations, particularly for larger displays.

**Specs:**

*   **Substrate Modification:**  Both first and second substrates (display layers) are manufactured with embedded microfluidic channel networks. These channels are extremely shallow (50-100um) and patterned to correspond to anticipated stress points, based on FEA modeling of the display during bending/use. Channel material: PDMS or similar flexible, chemically resistant polymer. Channels are sealed *within* the substrate material during manufacturing.
*   **Fluidic Interface:** Each substrate has micro-ports connected to the channel network. Ports are positioned along the perimeter and potentially at central stress locations.
*   **External Control System:** A pressure regulation system (manifold, pumps, sensors) is connected to the substrate ports.  This system allows for independent pressure control in each channel/region.
*   **Lamination Process:**
    1.  Adhesive is applied between substrates as per existing method.
    2.  Substrates are aligned and brought into contact.
    3.  Initial, uniform pressure is applied via external plates (as in the source patent).
    4.  *Dynamic Pressure Mapping:* The control system activates. Pressure sensors detect localized stress/voids. The control system adjusts pressure in individual channels to optimize adhesion and equalize stress. This is an iterative process, monitored in real time with sensors.
    5.  Once optimal adhesion is achieved, pressure is stabilized, and the system can remain active to maintain consistent pressure.
*   **Sensor Integration:**  Pressure sensors (piezoresistive or capacitive) are integrated *within* the microfluidic channels and/or bonded directly to the substrate surface to provide feedback to the control system.
*   **Materials:**  Substrate material: Flexible OLED, flexible glass, or suitable polymer. Adhesive: Optically clear, low-viscosity adhesive.
*   **Software Control:**
    ```pseudocode
    //Initialize
    connect_to_pressure_sensors()
    connect_to_fluidic_control_system()
    set_initial_pressure(uniform_pressure)

    //Main Loop
    while (adhesion_not_optimal) {
        sensor_data = read_pressure_sensors()
        stress_map = calculate_stress(sensor_data)

        //Identify high-stress/void areas
        problem_areas = identify_problem_areas(stress_map, threshold)

        //Adjust pressure in problem areas
        for each area in problem_areas {
            pressure_adjustment = calculate_adjustment(area, desired_pressure)
            apply_pressure(area, pressure_adjustment)
        }
        update_display()
    }
    set_stable_pressure(stable_pressure)
    ```

**Potential benefits:**

*   Eliminates voids and uneven adhesion.
*   Reduces stress concentrations and improves display durability.
*   Enables larger, more flexible displays.
*   Allows for precise control over display curvature.
*   Could be adapted for repair – selectively adjusting pressure to re-adhere delaminated areas.