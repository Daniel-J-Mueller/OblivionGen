# 8830174

## Variable Haptic Texture Display

**Concept:** Expand the variable profile button concept into a small-scale, dynamically reconfigurable tactile display. Instead of a single button, create an array of these electroactive polymer-driven "pixels" capable of creating raised textures on a surface.

**Specs:**

*   **Display Matrix:** 8x8 array of variable profile "tactile pixels". Scalable to larger sizes (16x16, 32x32, etc.).
*   **Pixel Dimensions:** Each pixel has a surface area of 2mm x 2mm in its retracted state. Maximum raised height of 1.5mm.
*   **Actuation:** Each pixel is driven by a dedicated dielectric electroactive polymer (DEAP) actuator, similar to the original patent. DEAP dimensions: 5mm x 5mm x 0.5mm. Target voltage: 250V – 400V.
*   **Substrate:** Flexible PCB substrate with embedded electrodes for individual pixel control. Material: Polyimide.
*   **Mechanical Linkage:** Each pixel utilizes a miniature "strut" system – four thin, flexible polymer rods connecting the DEAP to the pixel’s surface. These struts translate the DEAP’s expansion into vertical displacement of the pixel.
*   **Pixel Surface Material:** Smooth, durable polymer (e.g., silicone) with a Shore A hardness of 40. The surface is slightly textured to enhance tactile feel.
*   **Control System:**
    *   Microcontroller: ESP32-S3.
    *   Driver IC: Custom-designed high-voltage driver IC capable of individually addressing each pixel.
    *   Communication: Wi-Fi/Bluetooth for external control and data streaming.
*   **Power Requirements:** 5V DC for control circuitry, +300V DC for DEAP actuation (generated via a DC-DC converter).

**Pseudocode (Control Logic):**

```
// Function: updateDisplay(textureData[])
// Input: textureData[] - An array representing the desired texture.
//        Each element corresponds to a pixel's height (0-255).
// Output: None

function updateDisplay(textureData[]) {
  for (pixelIndex = 0 to textureData.length - 1) {
    pixelHeight = map(textureData[pixelIndex], 0, 255, 0, 1.5); // Map to pixel height in mm

    // Calculate voltage required for DEAP to achieve pixelHeight
    voltage = calculateVoltage(pixelHeight);

    // Send voltage command to corresponding pixel driver
    sendVoltageCommand(pixelIndex, voltage);
  }
}

// Function: calculateVoltage(pixelHeight)
// Input: pixelHeight - Desired height of the pixel in mm.
// Output: Voltage required for the DEAP to achieve the desired height.

function calculateVoltage(pixelHeight) {
  // Implement a lookup table or a mathematical function that maps pixel height to voltage.
  // This function needs to be calibrated based on the specific DEAP actuator.
  // For example:
  voltage = (pixelHeight / 1.5) * 400; // Linear mapping, assuming max 400V
  return voltage;
}

// Function: sendVoltageCommand(pixelIndex, voltage)
// Input: pixelIndex - Index of the pixel to control.
//        voltage - Voltage to apply to the pixel.
// Output: None

function sendVoltageCommand(pixelIndex, voltage) {
  // Send a command to the driver IC to apply the specified voltage to the corresponding pixel.
  // This requires communication with the driver IC using SPI or I2C.
}

// Main loop:
// 1. Receive texture data from external source (Wi-Fi, Bluetooth, etc.).
// 2. Call updateDisplay() to update the texture on the display.
```

**Potential Applications:**

*   **Braille displays:** Dynamically generate Braille characters for visually impaired users.
*   **Tactile maps:** Allow users to "feel" geographical features.
*   **Interactive art installations:** Create dynamic tactile surfaces for artistic expression.
*   **Haptic feedback for VR/AR:** Enhance immersion by providing realistic tactile sensations.
*   **Accessible user interfaces:** Provide tactile feedback for buttons and controls in mobile devices.