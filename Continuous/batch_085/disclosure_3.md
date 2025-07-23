# 10019029

## Variable Density Haptic Feedback Shell

**Concept:** Integrate microfluidic channels *within* the second injection molded layer to create a dynamically adjustable haptic feedback shell. This goes beyond simple vibration and allows for localized pressure changes and texture simulation on the device exterior.

**Specifications:**

*   **Layer 1 (Inner - Stiffness):**  High glass fiber content polycarbonate ( >40% ).  Provides structural rigidity and acts as the base for the microfluidic network. Molded directly onto the metal chassis as described in the source patent.
*   **Layer 2 (Outer - Haptic):** Polycarbonate with low glass fiber content ( <10% ), designed with a complex network of embedded microfluidic channels. Channels should be on the order of 0.2-0.5mm in diameter.
*   **Microfluidic System:**
    *   A small, internal pump and reservoir system (similar to cooling systems for high-end PCs) circulates a non-conductive, viscous fluid (silicone oil preferred) through the microfluidic channels.
    *   Electro-mechanical valves (miniature solenoid valves) control fluid flow to specific channel regions. These valves are controlled by the device’s processor.
    *   Channel density is *not* uniform. Higher density in areas intended for frequent haptic interaction (e.g., edges for grip, areas mimicking buttons).
    *   Channel shape can vary. Linear channels for directional pressure. Closed-loop channels for localized ‘bumps’ or texture changes.
*   **Software Control:**
    *   API for developers to create custom haptic feedback profiles.
    *   Haptic “shaders” – procedural generation of haptic patterns based on screen content. (e.g. running a finger across a textured surface on screen simulates the texture on the device back.)
    *   Adaptive haptic response based on user grip pressure and device orientation.
*   **Materials:**
    *   Layer 1: Polycarbonate + >40% Glass Fiber.
    *   Layer 2: Polycarbonate + <10% Glass Fiber. Channels formed *during* the injection molding process (channel geometry embedded into the mold).
    *   Fluid: Non-conductive silicone oil.
    *   Valves: Miniature solenoid valves (low power consumption).
*   **Manufacturing Process:**
    1.  Mold Layer 1 onto the metal chassis.
    2.  Design a multi-part mold for Layer 2, incorporating the microfluidic channel network.
    3.  Overmold Layer 2 onto Layer 1, embedding the channels.
    4.  Integrate the microfluidic pump, reservoir, and valves into the device housing.
    5.  Fluid filling and system testing.

**Pseudocode (Haptic Shader Example):**

```
function generateHapticPattern(textureImage, contactPoint) {
  // 1. Sample textureImage at contactPoint
  pixelColor = textureImage.getPixel(contactPoint.x, contactPoint.y);

  // 2. Map pixelColor to haptic intensity (0-100)
  hapticIntensity = map(pixelColor.brightness, 0, 255, 0, 100);

  // 3.  Select channel region closest to contactPoint
  channelRegion = findNearestChannel(contactPoint);

  // 4. Activate channelRegion with intensity hapticIntensity
  activateChannel(channelRegion, hapticIntensity);
}
```