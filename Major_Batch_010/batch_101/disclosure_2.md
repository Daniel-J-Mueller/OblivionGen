# D1059293

## Modular Smart Plug System with Environmental Sensing & Micro-Actuation

**Core Concept:** Expand the smart plug beyond simple on/off control to a modular, sensor-rich system capable of localized environmental adjustments.

**Module Types (interchangeable, magnetically attached):**

1.  **Air Quality Module:**
    *   Sensors: PM2.5, VOCs, CO2, Temperature, Humidity.
    *   Actuation: Miniature ultrasonic humidifier/dehumidifier, miniature activated carbon filter fan.
    *   Function: Localized air purification & humidity control near the plug.  Adjusts based on sensor data; can be overridden by user.
2.  **Light/Color Module:**
    *   Components: RGBW LED array, ambient light sensor.
    *   Actuation: Adjustable brightness/color temperature.
    *   Function: Creates localized accent lighting, color-changing mood lighting, or serves as a nightlight.  Can be synced with other smart devices.
3.  **Scent Diffusion Module:**
    *   Components: Miniature ultrasonic diffuser, refillable scent cartridge.
    *   Actuation: Variable intensity scent diffusion.
    *   Function: Dispenses aromatherapy oils or air fresheners locally. Controlled via app.
4.  **Micro-Irrigation Module:**
    *   Components: Miniature peristaltic pump, water reservoir, moisture sensor.
    *   Actuation: Delivers a controlled amount of water to a small plant or propagation setup.
    *   Function: Automates watering of small plants or seed starts.
5.  **Sound Module:**
    *   Components: Miniature speaker, microphone.
    *   Actuation: Plays pre-loaded sounds (white noise, nature sounds), or acts as a basic intercom.
    *   Function: Ambient sound masking or simple audio alerts.

**Plug Specifications:**

*   **Power:** Standard AC plug connection (US, EU, UK variants).  Handles up to 15A/1800W.
*   **Connectivity:** Wi-Fi 802.11 b/g/n, Bluetooth 5.0.
*   **Microcontroller:** ESP32-S3 or equivalent.
*   **Interface:**  Magnetic attachment point for modules.  Data communication via I2C/SPI.  LED indicator for status.
*   **Enclosure:**  Modular, 3D-printable housing.  Material: Flame-retardant ABS or PC.
*   **App Control:**  Dedicated mobile app for configuration, scheduling, remote control, and data visualization.  Integration with IFTTT and other smart home platforms.

**Pseudocode (Module Communication):**

```
// Main Plug Code

loop:
    scan for attached modules (I2C address range)
    for each module:
        read module ID
        request sensor data (I2C read)
        process data (apply user settings/schedules)
        send actuation commands (I2C write)
    wait 1 second

// Module Code (Example: Air Quality Module)

onAttach():
    advertise module ID via I2C

loop:
    read sensor data (PM2.5, VOCs, Temp, Humidity)
    if data exceeds threshold:
        activate humidifier/dehumidifier/fan
    respond to actuation commands from plug (I2C)
```

**Novelty:** Combines smart plug functionality with localized environmental control and customizable modules, exceeding the scope of typical smart plugs. Offers a versatile platform for various applications, from air purification to plant care.