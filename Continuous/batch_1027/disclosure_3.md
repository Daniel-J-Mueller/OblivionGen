# D577765

## Self-Folding Envelope System

**Concept:** An envelope incorporating micro-robotics and shape-memory alloys to autonomously fold and seal itself after receiving a letter, and subsequently unfold for reuse as a return envelope.

**Specs:**

1.  **Material:**
    *   Base Envelope: Constructed from a durable, flexible polymer film – Polyethylene Terephthalate (PET) or a similar material. Thickness: 0.15mm – 0.25mm.
    *   Micro-Robotic Components: Piezoelectric actuators, miniature gears, and flexible circuit boards. Material: Polydimethylsiloxane (PDMS) for flexibility and bio-compatibility.
    *   Shape-Memory Alloy (SMA) Struts: Nickel-Titanium alloy (NiTi). Diameter: 0.5mm. Pre-programmed deformation shapes for folding/unfolding.
    *   Adhesive Layer: Reusable, pressure-sensitive adhesive – silicone-based – applied to flap interior.

2.  **Actuation System:**
    *   Piezoelectric actuators embedded within the envelope’s flap and side panels. Number: 6-8 actuators.
    *   Actuators connected to miniature gears/linkages that control the flap’s folding motion.
    *   SMA struts strategically placed along the envelope’s edges to provide structural support during folding and unfolding.
    *   Microcontroller (integrated into the envelope) to sequence the activation of actuators and control SMA heating/cooling.
    *   Power Source: Thin-film battery (integrated into the envelope) – Lithium Polymer (LiPo) – Capacity: 50mAh. Rechargeable via induction. Alternatively, kinetic energy harvesting from the act of inserting a letter.

3.  **Folding/Unfolding Sequence:**
    *   *Letter Insertion Detection:* Capacitive sensor integrated within the envelope detects the presence of a letter.
    *   *Folding Initiation:* Microcontroller activates piezoelectric actuators and begins heating SMA struts.
    *   *Flap Folding:* Actuators drive the flap to fold over the envelope’s opening.
    *   *Side Panel Folding:* Actuators drive the side panels to fold inwards, securing the letter within the envelope.
    *   *Adhesive Sealing:* Pressure-sensitive adhesive on the flap secures the envelope shut.
    *   *Unfolding:* Upon receiving a specific signal (e.g., return address scan), the microcontroller reverses the actuator sequence, deactivates the SMA struts, and initiates unfolding.

4.  **Return Address Integration:**
    *   QR code or RFID tag embedded on the envelope's exterior.
    *   Scanning the QR code or RFID tag triggers the unfolding sequence.
    *   System checks for sufficient battery power before initiating unfolding.

5.  **Safety Mechanisms:**
    *   Overload protection circuit to prevent actuator damage.
    *   Temperature sensors to monitor SMA heating and prevent overheating.
    *   Emergency manual override switch to allow manual opening of the envelope.

**Pseudocode (Microcontroller Logic - Simplified):**

```
//Initialization
detectLetterInsertion = False
isFolded = False

//Main Loop
while (true) {
  if (letterDetected() && !isFolded) {
    foldEnvelope();
    isFolded = True;
  }
  if (returnAddressScanned() && isFolded) {
    if(batteryLevel > 20%){
       unfoldEnvelope();
       isFolded = False;
    } else {
        displayLowBatteryWarning();
    }
  }
}

function foldEnvelope() {
  activatePiezoActuators(flapFoldSequence);
  activateSMAS struts(foldConfiguration);
}

function unfoldEnvelope() {
  activatePiezoActuators(unfoldSequence);
  deactivateSMAS struts();
}
```