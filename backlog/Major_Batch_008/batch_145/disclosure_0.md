# 9754542

## Dynamic Pixel Aperture Control via Microfluidic Lens Arrays

**Concept:** Integrate a microfluidic lens array *above* the partition walls within each pixel to dynamically adjust the aperture and focal length, enhancing contrast and viewing angle. This moves beyond simple grayscale control to manipulate light *before* it reaches the viewer, creating a more immersive and realistic display.

**Specifications:**

*   **Substrate Modification:** Existing first substrate (described in patent) receives integrated microchannels. These channels are patterned *between* the insulating layer and the partition walls. Channels are formed using soft lithography or etching techniques compatible with existing manufacturing processes.
*   **Fluid Selection:** Electrowetting fluid (existing) is supplemented with a second, immiscible fluid with a high refractive index contrast. This secondary fluid fills the microchannels.
*   **Microchannel Array Design:** Each pixel contains a 2D array of microchannels (e.g., 5x5 or 10x10). Each microchannel is individually addressable via integrated microelectrodes.
*   **Electrode Configuration:** Microelectrodes are integrated into the first substrate *beneath* the microchannels. Applying a voltage to a specific electrode alters the surface tension of the secondary fluid within its corresponding microchannel, changing the lens shape.
*   **Lens Shape Control:** Microchannels are designed to deform into microlenses upon application of voltage. Deforming the microlens alters the aperture/focal length. A range of voltages enables a graded shift in shape and thus light modulation.
*   **Control Algorithm:**
    *   Pseudocode:

```
    FOR EACH Pixel:
        FOR EACH Microchannel:
            SET Voltage = CalculateVoltage(DesiredAperture, DesiredFocalLength, MicrochannelPosition)
    END FOR
    END FOR

    FUNCTION CalculateVoltage(DesiredAperture, DesiredFocalLength, MicrochannelPosition)
        // Map DesiredAperture & DesiredFocalLength to voltage range
        voltage = Map(DesiredAperture, MinAperture, MaxAperture, MinVoltage, MaxVoltage) +
                  Map(DesiredFocalLength, MinFocalLength, MaxFocalLength, MinVoltage, MaxVoltage)
        // Apply positional correction based on MicrochannelPosition (edge vs center)
        voltage += PositionalCorrection(MicrochannelPosition)
        RETURN voltage
    END FUNCTION
```

*   **Integration with Gate/Data Drivers:** The existing gate and data drivers are extended to include control signals for the microelectrode array. A new driver circuit would be responsible for applying individual voltages to each microelectrode.
*   **Second Substrate Modification:** The second substrate requires apertures to allow light transmission through the microlenses. These apertures can be integrated into the black matrix (existing).
*   **Material Compatibility:** All materials (fluids, electrodes, substrate) must be chemically compatible and prevent fluid leakage or contamination.
*   **Manufacturing Process:** Layered fabrication with lithographic patterning of microchannels and electrodes. Precise alignment is critical. Fluid filling performed after panel assembly.



This system goes beyond simple grayscale control. It enables dynamic control over the *direction* and *focus* of light emitted from each pixel, creating a more realistic and immersive display experience. The algorithm can also be adjusted for dynamic optimization of viewing angle.