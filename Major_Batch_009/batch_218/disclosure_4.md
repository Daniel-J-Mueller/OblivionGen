# 9400381

## Electrowetting Haptic Feedback System

**Concept:** Integrate microfluidic channels *within* the electrowetting display electrode structure to create localized pressure variations, enabling haptic feedback directly through the display surface. This goes beyond visual display to incorporate tactile sensation.

**Specifications:**

1.  **Electrode Fabrication:**
    *   Material: Indium Tin Oxide (ITO) or similar transparent conductive material.
    *   Layered Structure:
        *   Base ITO Layer: Forms the main conductive path for electrowetting.
        *   Microchannel Layer: A thin layer of photopolymer or similar material patterned with a network of interconnected microfluidic channels (channel width: 50-200µm, channel depth: 10-50µm). Channels should be filled with a non-conductive, viscous fluid (e.g., silicone oil).
        *   Top ITO Layer: A second ITO layer deposited on top of the microchannel layer, completing the electrode structure.  This top layer will be patterned with standard electrowetting pixel structures.
    *   Channel Interconnects: Each channel must connect to a dedicated micro-pump or pressure actuator.

2.  **Actuation System:**
    *   Micro-Pumps: Piezoelectric or MEMS-based micro-pumps are integrated beneath each channel (or groups of channels). These pumps will control fluid flow within the channels, creating localized pressure changes.  Pump resolution should allow for precise control of pressure (0.1 – 1 kPa).
    *   Control System: A microcontroller with dedicated drivers to control each micro-pump independently.  The microcontroller receives input from a touchscreen controller or other input device to determine which channels to actuate and with what intensity.

3.  **Display Integration:**
    *   Dielectric Layer: Standard dielectric layer applied over the entire electrode structure, including the microchannel network.
    *   Fluid Layer: Electrowetting fluid (oil/water mix) applied on top of the dielectric layer.
    *   Top Plate:  Transparent top plate to contain the electrowetting fluid and provide a touch surface.

4.  **Software/Control Algorithm:**
    *   Haptic Mapping:  Software to map visual elements on the display to corresponding haptic effects. For example, pressing a virtual button on the screen will actuate the micro-pumps beneath that button, creating a tactile ‘click’.
    *   Variable Actuation:  Ability to vary the pressure and duration of the haptic feedback to create different tactile sensations (e.g., a gentle ‘ripple’ for a water effect, a sharp ‘click’ for a button press).
    *   Pressure Calibration:  Software to calibrate the micro-pumps and ensure consistent haptic feedback across the entire display area.

**Pseudocode (Haptic Feedback Routine):**

```
FUNCTION GenerateHapticFeedback(x, y, effectType, intensity)
  // x, y: coordinates of the touch event
  // effectType: type of haptic feedback (e.g., "click", "ripple", "vibrate")
  // intensity: strength of the haptic feedback (0-100)

  // Calculate corresponding microchannel(s) based on x, y coordinates.
  channelList = GetChannelsForCoordinates(x, y)

  FOR EACH channel IN channelList
    // Determine appropriate pressure level and duration based on effectType and intensity.
    pressureLevel = CalculatePressureLevel(effectType, intensity)
    duration = CalculateDuration(effectType, intensity)

    // Send command to micro-pump to actuate channel with specified pressure and duration.
    ActivatePump(channel, pressureLevel, duration)
  END FOR
END FUNCTION
```

**Novelty:** This design goes beyond passive electrowetting displays by actively generating tactile feedback, enhancing the user experience. The integration of microfluidic channels *within* the electrode structure minimizes the impact on display resolution and transparency.