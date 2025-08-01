# 9547168

**Adaptive Pixel Wall Morphology via Microfluidic Control**

**Concept:** Implement pixel walls not as static structures, but as dynamically morphing features controlled by embedded microfluidic channels. This allows for optimization of shear stress mitigation *and* active control of pixel shape for improved viewing angles/contrast.

**Specs:**

*   **Material:** Pixel walls fabricated from a flexible polymer (e.g., PDMS) with integrated microfluidic channels.
*   **Channel Dimensions:** Channels ~50-100µm wide, running the length of each pixel wall. Density: 1 channel per 200µm of wall length.
*   **Fluid:** A low-viscosity, electrically insulating fluid (e.g., fluorinated oil) pumped through the microfluidic channels.
*   **Actuation:** Micro-pumps (MEMS-based) integrated into the display substrate to control fluid flow within each pixel wall. Each pump controlled by the display’s driving circuitry.
*   **Morphology Control Algorithm:**
    1.  **Shear Stress Sensing:** Integrate piezoresistive sensors *within* the pixel walls to measure shear stress in real-time.
    2.  **Dynamic Adjustment:** Based on sensor readings, the algorithm adjusts fluid flow in the adjacent microfluidic channels to actively deform the pixel walls.
    3.  **Deformation Modes:**
        *   **Bending:** Increase/decrease fluid pressure on one side of the wall to induce bending.
        *   **Expansion/Contraction:** Uniformly increase/decrease pressure to change wall thickness.
        *   **Localized Deformation:** Precisely control flow in specific channel segments to create complex wall shapes.
    4.  **Viewing Angle Optimization:** Algorithm incorporates a viewing angle map. By adjusting wall morphology, pixel orientation is optimized for the user’s current viewing position.
*   **Integration:**
    1.  Microfluidic channels are fabricated during the pixel wall creation process (e.g., soft lithography).
    2.  Micro-pumps and sensors are integrated into the display substrate using standard microfabrication techniques.
    3.  Fluid reservoirs and inlet/outlet ports are placed at the periphery of the display.
*   **Control System:** A dedicated control system regulates fluid flow and sensor data, interfacing with the display’s main processing unit.

**Pseudocode:**

```
// Main Loop
while (display_active) {
    // Read sensor data from each pixel wall
    for (each pixel_wall) {
        shear_stress = read_sensor(pixel_wall);
        // Calculate desired deformation based on shear stress and viewing angle
        desired_deformation = calculate_deformation(shear_stress, viewing_angle);
        // Calculate required fluid pressure for each channel segment
        fluid_pressure = calculate_pressure(desired_deformation);
        // Send pressure command to micro-pump
        send_command(micro_pump, fluid_pressure);
    }
}
```