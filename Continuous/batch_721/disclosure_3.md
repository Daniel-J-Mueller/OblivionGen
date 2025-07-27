# 11208270

## Adaptive Resonance Frequency Dimensioning

**Concept:** Augment the light interruption method with resonant frequency analysis to determine item dimensions, especially for non-rigid or deformable objects.

**Specifications:**

1.  **Resonant Frequency Emitters/Receivers:** Integrate pairs of micro-scale resonant frequency emitters and receivers *within* the light emitting and receiving arrays. These operate at a range of pre-defined frequencies (e.g., 100Hz – 10kHz).
2.  **Material Profile Database:** Establish a database linking resonant frequency response to material type and density. This is populated empirically.
3.  **Emitter Sweep:** Upon item passage, the emitters sweep through the pre-defined frequency range.
4.  **Resonance Detection:** Receivers detect resonant frequencies induced in the item as it interrupts the emitted waves.
5.  **Data Fusion:** The system fuses data from:
    *   Light interruption time (as in the base patent).
    *   Detected resonant frequencies.
    *   Material profile from database (identified by pre-scan or object recognition).
6.  **Dimension Calculation:** A processing unit calculates item dimensions using:
    *   Light interruption time to establish a baseline dimension.
    *   Resonant frequency data to refine the dimension accounting for material deformation or flexibility. The resonant frequency influences the baseline dimension calculated by light interruption.
    *   Material profile to correct for density variations and resonant behavior.

**Pseudocode:**

```
FUNCTION CalculateDimension(lightInterruptionTime, resonantFrequencies, materialProfile)

    baselineDimension = CalculateBaselineDimension(lightInterruptionTime)

    resonantCorrectionFactor = CalculateResonantCorrection(resonantFrequencies, materialProfile)

    correctedDimension = baselineDimension * resonantCorrectionFactor

    RETURN correctedDimension
END FUNCTION

FUNCTION CalculateResonantCorrection(resonantFrequencies, materialProfile)

    //Access material properties from the materialProfile database
    materialDensity = materialProfile.density
    materialYoungsModulus = materialProfile.youngsModulus

    //Calculate the expected resonant frequency based on material properties and geometry
    expectedResonantFrequency = CalculateExpectedFrequency(materialDensity, materialYoungsModulus)

    //Compare expected and measured frequencies
    frequencyDifference = measuredResonantFrequency – expectedResonantFrequency

    //Adjust correction factor based on frequency difference
    correctionFactor = 1 + (frequencyDifference * sensitivityFactor)

    RETURN correctionFactor
END FUNCTION

FUNCTION CalculateBaselineDimension(lightInterruptionTime)
    //Standard calculation using conveyor speed as in the patent
    baselineDimension = conveyorSpeed * lightInterruptionTime
    RETURN baselineDimension
END FUNCTION
```

**Hardware Components:**

*   Micro-scale resonant frequency emitters (piezoelectric or MEMS-based).
*   High-sensitivity resonant frequency receivers.
*   Dedicated signal processing unit for resonance detection.
*   Expanded material profile database.

**Potential Applications:**

*   Dimensioning of soft or deformable items (e.g., pillows, clothing).
*   Improving accuracy for irregular shaped objects.
*   Automatic material identification.