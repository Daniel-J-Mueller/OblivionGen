# D707123

## Adaptive Hinge & Lock System for Box Closure Flaps

**Concept:** Integrate a flexible, shape-memory alloy (SMA) hinge *within* the box closure flap itself, coupled with a micro-actuated locking mechanism. This allows for automated, dynamic adjustment of the flap's closing force and security level.

**Specs:**

*   **Hinge Material:** Nickel-Titanium (NiTi) SMA wire/mesh embedded within the flap’s material (e.g., cardboard, plastic). Strand diameter: 0.3mm - 0.5mm. Mesh density customizable based on flap size and desired flexibility.
*   **Actuation:** Low-voltage (3.3V - 5V) pulse-width modulation (PWM) control to regulate current flow through the SMA, inducing a change in its shape (bending/straightening).  A small onboard microcontroller (e.g., ATTiny85) manages PWM signal.
*   **Locking Mechanism:** Miniature solenoid-actuated pin/claw system integrated into the flap’s leading edge. Pin material: hardened plastic or lightweight alloy.  Actuation voltage: 3.3V – 5V, controlled by the same microcontroller as the SMA.
*   **Sensor Integration:**  Force-sensitive resistor (FSR) embedded within the flap to detect closing pressure. Data fed back to the microcontroller for adaptive force adjustment.
*   **Power:** Rechargeable thin-film battery integrated into the flap (dimensions customizable). Wireless charging capability (Qi standard).
*   **Communication:** Bluetooth Low Energy (BLE) module for remote control and status monitoring (e.g., battery level, lock status).
*   **Flap Material:** Compatible with standard cardboard, corrugated plastic, or thin polymer sheets. Embedding the SMA/electronics requires slight material reinforcement in those areas.
*   **Dimensions:** System footprint should add no more than 2mm to the overall flap thickness.

**Pseudocode (Microcontroller Logic):**

```
// Define Pins
const int smaPin = 2;  // SMA Actuation Control
const int lockPin = 3; // Lock Actuation Control
const int sensorPin = A0; // Force Sensor Analog Input
const int blePin = 4; // BLE Communication

// Variables
int sensorValue = 0;
int desiredForce = 500; // Example value - adjustable via BLE

void setup() {
  pinMode(smaPin, OUTPUT);
  pinMode(lockPin, OUTPUT);
  Serial.begin(9600); // For debugging
  // Initialize BLE
}

void loop() {
  // Read Sensor Value
  sensorValue = analogRead(sensorPin);

  // Calculate Adjustment
  int adjustment = desiredForce - sensorValue;

  // Adjust SMA Activation
  if (adjustment > 0) {
    // Increase SMA force - higher PWM duty cycle
    analogWrite(smaPin, map(adjustment, 0, 1000, 0, 255));
  } else {
    // Decrease SMA force - lower PWM duty cycle
    analogWrite(smaPin, map(abs(adjustment), 0, 1000, 0, 255));
  }

  // Check Lock Status (toggle via BLE)
  if (lockStatus == LOCKED) {
    digitalWrite(lockPin, HIGH); // Activate Lock
  } else {
    digitalWrite(lockPin, LOW); // Deactivate Lock
  }

  delay(100);
}
```

**Functionality:** The system dynamically adjusts the closing force of the flap based on the contents and desired seal.  Users can remotely control the lock status and adjust the target force via a smartphone app.  This creates a secure and adaptable packaging solution for sensitive items.