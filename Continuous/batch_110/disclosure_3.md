# D590440

## Dynamic Embossing & Micro-Fluidic Integration - Envelope System

**Concept:** An envelope system integrating dynamic embossing and micro-fluidic channels to deliver a scent or small particulate (e.g., glitter, dried flower petals) upon opening. The embossing isn’t static; it *changes* based on user interaction or environmental factors.

**Specs:**

1.  **Substrate:** Multi-layer material:
    *   Outer layer: Durable, printable polymer (Polypropylene, PET).
    *   Intermediate layer: Micro-fluidic channel network etched into a biocompatible polymer (PDMS or similar). Channels are sealed.
    *   Inner layer: Absorbent material containing scent/particulate reservoir.

2.  **Dynamic Embossing System:**
    *   Piezoelectric actuators integrated beneath the outer layer.  Actuators arranged in a grid pattern allowing for localized deformation of the polymer.
    *   Microcontroller (integrated within envelope - coin cell battery powered) controlling actuator array.
    *   Sensor suite:
        *   Bend sensor (detects envelope opening).
        *   Temperature sensor (environmental temperature trigger).
        *   Light sensor (detects ambient light level).
        *   Optional: Capacitive touch sensor (user touch trigger).
    *   Firmware: Logic controlling actuator array based on sensor input.  Example: Upon detecting envelope opening *and* temperature above 20°C, activate actuators to create a raised "flower bloom" embossing pattern while simultaneously dispensing scent from the micro-fluidic channels.

3.  **Micro-Fluidic System:**
    *   Reservoir: Small, sealed chamber containing liquid scent or particulate suspended in a carrier fluid.
    *   Channel Network: Etched channels connecting reservoir to dispensing points aligned with embossing pattern (e.g., "petals" of embossed flower).
    *   Micro-Pump: Piezoelectric micro-pump to regulate fluid/particulate flow through channels, triggered by microcontroller.
    *   Dispensing Mechanism:  Micro-pores at channel endpoints to allow scent/particulate to diffuse through the outer layer.

4.  **Embossing Pattern Library:**  Microcontroller stores a library of pre-programmed embossing patterns (flower, geometric shapes, custom logos). User personalization via external programming (Bluetooth connection potentially)

**Pseudocode (Microcontroller Firmware):**

```
// Define sensor pins and actuator array

// Initialize sensors and actuators

loop:
  bendSensorValue = readBendSensor()
  tempValue = readTemperatureSensor()
  lightValue = readLightSensor()

  if (bendSensorValue > threshold AND tempValue > 20 AND lightValue < 50) then
    activateEmbossingPattern("flower")
    activateMicroPump()
    delay(5000) // Hold pattern/dispense for 5 seconds
    deactivateMicroPump()
    deactivateEmbossingPattern()
  else if (bendSensorValue > threshold AND tempValue < 10) then
    activateEmbossingPattern("snowflake")
    activateMicroPump()
    delay(5000)
    deactivateMicroPump()
    deactivateEmbossingPattern()
  else
    maintainDefaultState()
  end if
end loop
```

**Variations:**

*   Replace scent/particulate with conductive ink to create a touch-sensitive surface.
*   Integrate a small OLED display within the embossed pattern to show custom messages.
*   Use a haptic feedback system alongside the dynamic embossing for a more immersive experience.