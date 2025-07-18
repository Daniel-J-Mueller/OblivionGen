# D768769

## Interactive Gift Card Constellation

**Concept:** A gift card assembly featuring a peel-away gift card, but instead of a simple sticker/tear strip, incorporates a network of conductive traces embedded within a flexible substrate. This substrate connects to a small, coin-cell powered microcontroller and a series of miniature, surface-mount LEDs. The 'sticker' portion isn’t just decorative; it *is* the interactive circuit.

**Materials:**

*   **Gift Card Base:** Standard cardstock material, potentially with a metallic layer for aesthetic appeal and grounding.
*   **Flexible Substrate:** Polyimide or similar flexible PCB material, approximately 0.1mm thick.
*   **Conductive Ink/Traces:** Silver or copper-based conductive ink, printed onto the flexible substrate to create a network of interconnected pathways.
*   **Microcontroller:** Tiny, low-power microcontroller (e.g., ATTiny85 or similar) with minimal I/O pins.
*   **Surface Mount LEDs:** Miniature, multi-color LEDs (RGB possible) – approximately 1mm x 1mm.
*   **Coin Cell Battery:** 3V coin cell battery (CR2032 or similar).
*   **Peel-Away Adhesive:**  Specially formulated adhesive allowing for clean removal of the flexible substrate from the gift card base without damaging the circuits.
*   **Protective Overcoat:** Clear, flexible coating to protect the conductive traces and LEDs from damage and moisture.

**Assembly & Functionality:**

1.  **Circuit Layout:** Design a network of interconnected conductive traces on the flexible substrate. These traces should form a "constellation" pattern, with each LED representing a "star". The traces should be designed to light up in a sequence or pattern when the sticker is peeled away from the base card.
2.  **Component Mounting:** Mount the microcontroller and LEDs onto the flexible substrate using conductive adhesive or solder.
3.  **Battery Connection:** Integrate the coin cell battery into the flexible substrate, establishing a secure electrical connection to the microcontroller.
4.  **Adhesive Application:** Apply the peel-away adhesive to the back of the flexible substrate.
5.  **Attachment to Gift Card Base:** Carefully align and attach the flexible substrate (with adhesive) to the gift card base.
6.  **Protective Overcoat:** Apply the protective overcoat to the entire assembly, ensuring all circuits are sealed and protected.

**Pseudocode (Microcontroller Logic):**

```
// Define LED pins
const int led1Pin = 0;
const int led2Pin = 1;
const int led3Pin = 2;
// ... and so on for each LED

// Peel Detection: Assume a dedicated pin connected to a trace
// that completes a circuit when the sticker is removed.
const int peelPin = 3;

void setup() {
  pinMode(peelPin, INPUT_PULLUP); // Internal pull-up resistor
  pinMode(led1Pin, OUTPUT);
  pinMode(led2Pin, OUTPUT);
  //... and so on for each LED
}

void loop() {
  if (digitalRead(peelPin) == LOW) { // Sticker has been peeled (circuit completed)
    // Run animation sequence
    digitalWrite(led1Pin, HIGH);
    delay(100);
    digitalWrite(led2Pin, HIGH);
    delay(100);
    // ... continue sequencing through LEDs
    // Add more complex patterns, fading, color changes etc.
  } else {
    // All LEDs off when sticker is still attached
    digitalWrite(led1Pin, LOW);
    digitalWrite(led2Pin, LOW);
    // ... all LEDs low
  }
}
```

**Variations:**

*   **NFC/RFID Integration:** Integrate a small NFC/RFID tag into the flexible substrate to allow for digital interaction with the gift card (e.g., unlocking bonus content, tracking usage).
*   **Pressure Sensitivity:** Incorporate pressure sensors into the flexible substrate to trigger different LED patterns or effects when the sticker is pressed.
*   **Customizable Animation:** Allow users to customize the LED animation sequence via a smartphone app.
*   **Multiple Layers:** Create multi-layered flexible substrates with more complex circuit designs and LED patterns.