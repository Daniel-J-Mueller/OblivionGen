# 10078213

## Microfluidic Lens Arrays for Dynamic Focus Electrowetting Displays

**Concept:** Integrate microfluidic lens creation *within* the spacer grid fabrication process for electrowetting displays. Instead of solely creating ducts for fluid movement *between* pixels, the spacer grid will define micro-chambers capable of altering shape via fluidic pressure, effectively creating dynamically adjustable lenses *above* each pixel. This enables a novel method for achieving focused or blurred regions on the display *without* needing complex optical elements.

**Specs:**

*   **Spacer Material:** A two-layer system.
    *   **Base Layer:** A rigid, transparent polymer (e.g., PMMA, Cyclic Olefin Copolymer) forming the main structural support and defining the duct/channel network.
    *   **Actuation Layer:** A flexible, sealed micro-chamber layer deposited on top of the base layer, defining individual lens cells. Material: PDMS or a similar elastomeric polymer. This layer must be chemically compatible with the display fluid and transparent.
*   **Micro-Chamber Dimensions:** 
    *   Diameter: 50-200 μm (pixel-dependent).
    *   Height (when deflated): 5-10 μm.
    *   Wall thickness: 1-2 μm.
*   **Fluidic Interconnects:** Micro-channels within the base layer connect each micro-chamber to a common fluid reservoir and a micro-pump/valve system (integrated onto the support plate, or external). These channels are separate from the pixel switching fluid channels.
*   **Fabrication Process Adaptation:**
    1.  **Base Layer Fabrication:** Fabricate the primary spacer grid as described in the provided patent, creating the duct network.
    2.  **Micro-Chamber Patterning:** Utilize a second mask layer to define circular openings *within* the spacer walls for each pixel. These openings will become the micro-chambers.
    3.  **Actuation Layer Deposition:** Deposit the flexible actuation layer (PDMS) over the entire structure.  Ensure complete filling of the patterned openings.
    4.  **Sealing/Bonding:** Seal the PDMS layer to the base layer via plasma bonding or a compatible adhesive.
    5.  **Fluidic Connection:** Etch micro-channels extending from each micro-chamber to the external fluid reservoir/pump.
*   **Control System:**
    *   Micro-pump/Valve System:  Precise control of fluid flow into/out of each micro-chamber.
    *   Pressure Sensors: Monitor pressure within each chamber for closed-loop control.
    *   Control Algorithm:  Algorithm to dynamically adjust pressure in each chamber to create desired lens shapes (convex, concave, flat) and focal lengths.

**Pseudocode (Control Algorithm):**

```
// Input:  Pixel coordinates (x, y), desired focal length (f)
// Output: Pressure value for micro-chamber at (x, y)

function calculate_pressure(x, y, f) {
  // Define chamber radius (r)
  r = chamber_radius

  // Calculate ideal chamber height (h) based on desired focal length
  // (This is a simplified model - ray tracing or finite element analysis may be needed for accuracy)
  h = f / (2 * r) // Approximation for thin lens

  // Calculate pressure needed to achieve height 'h'
  // (This requires calibration and characterization of chamber elasticity)
  pressure = k * (h - initial_height)  // k is a spring constant

  // Apply pressure limits
  if (pressure > max_pressure) {
    pressure = max_pressure
  } else if (pressure < min_pressure) {
    pressure = min_pressure
  }

  return pressure
}
```

**Potential Benefits:**

*   **Dynamic Focusing:** Allows for real-time adjustment of focus without mechanical components.
*   **3D Display Effects:** Enables the creation of pseudo-3D effects by selectively focusing different regions of the display.
*   **Improved Contrast:** Localized focusing can enhance contrast in specific areas.
*   **Novel Display Modes:** Allows for the creation of displays that adapt to user viewing habits or environmental conditions.