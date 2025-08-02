# 10234678

## Adaptive Viscosity Dispensing for Electrowetting Element Manufacture

**Concept:** To dynamically control the viscosity of the dispensed fluid *during* the layering process, not just through fluid selection. This allows for finer feature control and potentially eliminates the need for precise evaporation steps.

**Specs:**

*   **Fluid System:** Two primary fluid reservoirs. Reservoir A: The primary 'electrowetting' fluid. Reservoir B: A secondary, miscible fluid with a significantly different viscosity and controllable addition rate.
*   **Microfluidic Mixing Head:** A custom-designed microfluidic head integrated with the dispensing system. This head rapidly mixes fluids from Reservoir A and Reservoir B in precise ratios *immediately* before dispensing. Mixing is achieved using a chaotic micromixer design to ensure homogeneity.
*   **Viscosity Sensor:** An inline micro-viscometer integrated into the microfluidic mixing head. This sensor continuously measures the viscosity of the fluid mixture.
*   **Closed-Loop Control System:** A PID controller that receives viscosity readings from the micro-viscometer and adjusts the ratio of fluids from Reservoir A and Reservoir B to maintain a target viscosity profile during dispensing.
*   **Support Plate with Integrated Heating/Cooling:** The support plate will have integrated micro-heaters and coolers to precisely control the temperature of the dispensed fluid *after* dispensing. This helps with uniform layer formation and solvent control.
*   **Layering Mode Selection:** System can operate in:
    *   **Constant Viscosity Mode:** Maintain a constant viscosity throughout the entire layer deposition.
    *   **Gradient Viscosity Mode:** Implement a viscosity gradient across the layer – potentially useful for creating specific fluidic geometries.
    *    **Feedback Mode:** Layer viscosity is determined by a sensor on the deposition surface.

**Pseudocode:**

```
// Initialization
define TargetViscosityProfile // an array of viscosity values based on X,Y coordinates
define InitialFluidRatio = 0.9 (Reservoir A) / 0.1 (Reservoir B)
define MaxFluidRatioA = 0.99
define MinFluidRatioA = 0.01

// Main Loop
while (LayerNotComplete) {
    Read TargetViscosity at current X,Y coordinate
    Measure CurrentViscosity with Inline Viscometer
    Error = TargetViscosity - CurrentViscosity

    Adjust FluidRatioA = FluidRatioA + (Error * Gain)  // PID control

    // Clamp FluidRatioA between Min and Max values

    Set Dispensing Flow Rate for Reservoir A and Reservoir B based on Adjusted FluidRatio

    Dispense Fluid

    Move to next X,Y coordinate
}
```

**Novelty:** Current methods rely primarily on pre-defined fluid properties and evaporation control. This system allows for *real-time* adjustment of fluid properties during deposition, enabling greater control over layer thickness, uniformity, and feature definition. The feedback mode represents a truly adaptive deposition process.