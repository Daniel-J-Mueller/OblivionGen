# 11858179

## Variable Density Mandrel with Integrated Sensor Network

**Concept:** Extend the thermally expanding mandrel concept by incorporating variable density materials and an embedded sensor network to actively control expansion and monitor component formation *during* the curing process.

**Specifications:**

*   **Mandrel Material:** Multi-material composition. Core comprised of high thermal expansion material (micronized rubber/silicone, as in the source patent). Surrounding layers constructed from materials with progressively *lower* thermal expansion coefficients, layered concentrically. Layers may be constructed from polymers or ceramics. Each layer is capable of independent temperature control.
*   **Density Gradient:** The mandrel’s density is not uniform. Density *decreases* radially outwards from the core. This facilitates more controlled expansion and minimizes stress concentration during component curing.
*   **Sensor Network:** Integrated micro-sensors (temperature, pressure, strain) embedded within the mandrel’s core and intermediate layers. Sensors are wirelessly connected (Bluetooth Low Energy) to an external control unit.
*   **Control Unit:** Software-defined control unit. User interface allows setting desired component geometry, material properties, and curing parameters. Real-time sensor data is used to adjust heating rates for each mandrel layer, providing dynamic control over the expansion process.
*   **Mandrel Fabrication:** Additive manufacturing (3D printing) is preferred. Allows precise control over material composition, layer thickness, and sensor placement. Alternatively, the mandrel may be constructed via layered molding or machining.
*   **Heating System:** The mandrel incorporates micro-resistive heating elements within each layer, enabling localized temperature control. Power supply is external, with safety interlocks to prevent overheating.
*   **Fracture/Dissolution Enhancement:** The outer layers are engineered to preferentially fracture or dissolve upon contact with water, aiding mandrel removal. Incorporate micro-voids or water-soluble additives into the outer layer material.
*   **Layer Composition Example:**
    *   **Core:** 70% micronized rubber, 30% gypsum plaster (high thermal expansion) + embedded heating elements and sensors.
    *   **Layer 1:** 50% polymer blend (moderate thermal expansion), 50% ceramic powder.
    *   **Layer 2:** 80% ceramic powder, 20% polymer blend.
    *   **Outer Layer:** Water-soluble polymer with integrated micro-voids.

**Pseudocode (Control Algorithm):**

```
// Initialization
set TargetGeometry = UserDefinedGeometry
set MaterialProperties = UserDefinedMaterialProperties
set CuringParameters = UserDefinedCuringParameters

// Main Loop
while (ComponentNotCured) {
    // Read Sensor Data
    temperature = ReadTemperatureSensors()
    pressure = ReadPressureSensors()
    strain = ReadStrainSensors()

    // Calculate Expansion Deviation
    deviation = CalculateExpansionDeviation(TargetGeometry, strain)

    // Adjust Heating Rates
    heatingRates = CalculateHeatingRates(deviation, temperature, CuringParameters)

    // Apply Heating Rates to Mandrel Layers
    ApplyHeatingRates(heatingRates)

    // Log Data
    LogData(temperature, pressure, strain, heatingRates)
}

// Post-Curing
// Activate dissolution/fracture of outer layer
ActivateDissolution()

```

**Potential Benefits:**

*   Increased geometric accuracy and control over component formation.
*   Ability to manufacture components with complex geometries and varying wall thicknesses.
*   Real-time monitoring and adjustment of the curing process.
*   Reduced material waste and improved component quality.
*   Potential for automating the manufacturing process.