# 9647337

## Dual-Band Metamaterial Antenna Array

**Concept:** Leverage the dual-band characteristics described in the reference patent to create a phased array antenna utilizing metamaterial elements for beam steering and enhanced gain. This moves beyond a single antenna to a system capable of dynamic signal direction.

**Specs:**

*   **Array Configuration:** 8x8 planar array. Element spacing = 0.5λ at the lower frequency band (2.4-2.5 GHz).
*   **Element Type:** Modified version of the dual-band patch antenna from the reference patent. Each element incorporates a split-ring resonator (SRR) metamaterial layer *above* the patch.
*   **SRR Dimensions:** SRR outer diameter = 5mm, gap width = 0.5mm, SRR material = copper. SRR is suspended 2mm above the patch using a dielectric spacer (εr = 4).
*   **Patch Dimensions:** 20mm x 25mm. Material: FR4 substrate.
*   **Feeding Network:** Corporate feed network with 64 individual phase shifters (one per element). Phase shifters operate between 0 and 360 degrees. Controlled by a microcontroller.
*   **Frequency Bands:** 2.4-2.5 GHz and 7.0-8.0 GHz.
*   **Beam Steering:** Electronic beam steering in both azimuth and elevation.
*   **Polarization:** Linear polarization (can be extended to dual polarization with additional elements).

**Pseudocode (Beam Steering Algorithm):**

```
// Input: Desired Azimuth Angle (AzAngle), Desired Elevation Angle (ElAngle), Frequency (Freq)
// Output: Phase Shift Values for each antenna element

function calculatePhaseShifts(AzAngle, ElAngle, Freq):
  // Calculate wavelength at Freq
  wavelength = speedOfLight / Freq

  // Calculate phase shift gradient based on AzAngle and ElAngle
  phaseGradientAzimuth = (AzAngle / 360) * 2 * PI
  phaseGradientElevation = (ElAngle / 360) * 2 * PI

  // Calculate the x and y coordinates for each antenna element
  for (i = 0 to 7):
    for (j = 0 to 7):
      x = i * elementSpacing
      y = j * elementSpacing

      //Calculate the phase shift for each element
      phaseShift = (phaseGradientAzimuth * x) + (phaseGradientElevation * y)

      // Normalize phase shift to 0-360 degrees
      phaseShift = phaseShift % (2 * PI)

      // Assign phase shift value to element [i][j]
      elementPhaseShifts[i][j] = phaseShift

  return elementPhaseShifts
```

**Innovation Details:**

The incorporation of the SRR metamaterial elements *above* the existing dual-band patch antenna serves several purposes. First, it enhances the impedance matching at higher frequencies (7.0-8.0 GHz), providing increased gain. Second, the SRR's resonant frequency can be tuned by changing its geometry, offering a method for fine-tuning the antenna's performance.  The phased array configuration enables dynamic beam steering, allowing the antenna to focus its energy in a specific direction.  This configuration provides improved range and reduced interference.