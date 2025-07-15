# 10018828

## Electrowetting Array with Dynamic Micro-Channel Formation

**Concept:** A display leveraging electrowetting, but incorporating dynamically formed micro-channels within the fluid layer *above* the active electrowetting elements. These channels would not be etched into a substrate, but created and sustained via a secondary electrowetting effect utilizing a third, highly viscous fluid, allowing for pixel-level light guidance and enhanced contrast.

**Specs:**

*   **Layer 1: Substrate & First Electrode Array:** Standard transparent substrate with patterned ITO electrodes for core electrowetting function (controlling primary fluid 1 & 2). Resolution target: 100 PPI.
*   **Layer 2: Primary Fluid 1 (Light Emitting):**  A highly emissive fluid (e.g., a microparticle suspension in a clear carrier) capable of being driven to emit light when an electric field is applied.  Viscosity: 1-5 cP.  Refractive Index: 1.4 - 1.6.
*   **Layer 3: Primary Fluid 2 (Immiscible):**  An immiscible fluid acting as a ‘blocker’ for Fluid 1, forming the basis of the pixel structure. Viscosity: 1-5 cP. Dielectric Constant: Low (<3).
*   **Layer 4: Channel Fluid (Viscous, Electrowetting Active):** A highly viscous, transparent fluid (e.g., silicone oil with dissolved conductive nanoparticles) acting as the channel former. Viscosity: 50-100 cP. Dielectric Constant: Moderate (3-5). This fluid exists in a separate, sealed micro-reservoir above the primary fluid layer and is delivered via micro-fluidic channels.
*   **Layer 5: Channel Electrode Array:** A second, patterned ITO array, orthogonal to the first, controlling the movement and formation of the Channel Fluid.  Electrode pitch: 50-100um.
*   **Layer 6: Top Plate/Sealing Layer:** Transparent top plate sealing the system and providing mechanical support.

**Operation:**

1.  **Baseline State:** Both electrode arrays are inactive. Primary Fluid 1 is dispersed within Fluid 2. The Channel Fluid reservoir is full.
2.  **Channel Formation:**  Voltage applied to a specific Channel Electrode. This attracts Channel Fluid downwards, forming a narrow micro-channel *above* the activated pixel area. The channel's width is controlled by the voltage applied.
3.  **Light Guiding:** Voltage applied to the primary pixel electrode.  Fluid 1 is pulled into the area defined by the micro-channel.  The channel walls act as a light guide, confining the emitted light and increasing contrast.
4.  **Dynamic Control:** By dynamically controlling both electrode arrays, the micro-channels (and therefore the light emission) can be shaped and positioned with sub-pixel accuracy.  Complex grayscale and color images are created by modulating both the electrowetting and channel fluid control.
5.  **Fluid Circulation:** Micro-pumps (integrated into the device) would be needed to circulate the channel fluid, replenishing areas where fluid is drawn down and preventing long-term depletion.

**Pseudocode (Channel Formation Control):**

```
FUNCTION create_channel(pixel_x, pixel_y, channel_width, channel_length):
  // pixel_x, pixel_y: Coordinates of the desired channel center
  // channel_width: Desired width of the channel (in um)
  // channel_length: Desired length of the channel (in um)

  FOR each Channel Electrode (x, y) within a radius of (channel_width / electrode_pitch) from (pixel_x, pixel_y):
    voltage = calculate_voltage(distance(x, y, pixel_x, pixel_y), channel_width)
    SET_VOLTAGE(Channel Electrode (x, y), voltage)
  END FOR
END FUNCTION

FUNCTION calculate_voltage(distance, channel_width):
  // Voltage calculation based on distance and desired channel width
  // Algorithm: Inverse square law with scaling factor
  voltage = (scaling_factor / (distance^2)) * channel_width
  RETURN voltage
END FUNCTION

```

**Potential Benefits:**

*   Increased contrast and brightness due to light confinement.
*   Sub-pixel control over light emission.
*   Dynamic shaping of pixel structures.
*   Potential for 3D displays by controlling channel height.