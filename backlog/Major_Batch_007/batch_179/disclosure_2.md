# D966194

## Adaptive Geometry Plug Adapter

**Concept:** A plug adapter that dynamically adjusts its prong configuration to match various outlet types *without* mechanical moving parts. This is achieved through a malleable, shape-memory alloy (SMA) core within each prong, controlled by micro-heating elements.

**Specs:**

*   **Material:** Core – Nickel-Titanium SMA (Nitinol). Exterior – High-temperature, electrically insulating polymer (PEEK or similar).
*   **Prong Configuration:** Three or more prongs, each containing a central SMA core and an outer polymer shell.
*   **Heating Elements:** Micro-resistive heating elements embedded *within* the SMA core, individually addressable. Element size: 0.5mm x 1mm x 2mm. Power consumption per element: 0.5W max.
*   **Control System:** A miniature microcontroller (ESP32 or similar) integrated into the adapter body.  
    *   Input:  Voltage detection (to determine operating voltage and potentially country).
    *   Logic: Based on detected voltage *and* a user-selected country (via a small button/switch or Bluetooth app), the microcontroller activates specific heating elements within each prong.
    *   Output:  Pulse Width Modulation (PWM) to control current flow to the heating elements.
*   **Power Source:** Adapter harvests power from the connected device via a step-down converter.  Minimum voltage: 5V.
*   **Dimensions:** Overall size comparable to existing plug adapters.
*   **Safety Features:**
    *   Over-temperature protection: Thermistor integrated into each prong to shut down heating if a set temperature is exceeded (85°C).
    *   Short-circuit protection:  Integrated fuse.
    *   Insulation:  Double-layered insulation to prevent electrical shock.

**Operation:**

1.  The user selects the destination country (optional, if voltage detection is reliable).
2.  The adapter detects the input voltage.
3.  The microcontroller activates specific heating elements in each prong, causing the SMA core to deform and change the prong's shape to match the target outlet type.
4.  Upon removal from the outlet, the SMA core cools and returns to its default shape.

**Pseudocode:**

```
FUNCTION initialize()
  set microcontroller pins for heating elements, voltage detection, and country selection
  set default country to 'auto'
END FUNCTION

FUNCTION readVoltage()
  read analog voltage from voltage detection pin
  RETURN voltage
END FUNCTION

FUNCTION readCountrySelection()
  IF button pressed or Bluetooth signal received
    RETURN selected country
  ELSE
    RETURN 'auto'
  END IF
END FUNCTION

FUNCTION determineProngConfiguration(voltage, country)
  IF country == 'auto'
    IF voltage between 100-127V
      RETURN 'US_Configuration'
    ELSE IF voltage between 220-240V
      RETURN 'EU_Configuration'
    ELSE
      RETURN 'Default_Configuration'
    END IF
  ELSE
    //Lookup table for country-specific prong configurations
    RETURN lookupTable[country]
  END IF
END FUNCTION

FUNCTION activateHeatingElements(configuration)
  //Based on configuration, activate specific heating elements in each prong.
  //Example:
  IF configuration == 'US_Configuration'
    prong1.heatingElement1 = ON
    prong2.heatingElement2 = ON
    prong3.heatingElement3 = OFF
  END IF
END FUNCTION

FUNCTION main()
  initialize()
  WHILE(TRUE)
    voltage = readVoltage()
    country = readCountrySelection()
    configuration = determineProngConfiguration(voltage, country)
    activateHeatingElements(configuration)
  END WHILE
END FUNCTION
```

**Novelty:**

Current plug adapters rely on mechanical pivoting or sliding prongs. This design removes moving parts, increasing reliability and reducing complexity. The use of shape-memory alloys and precise temperature control allows for dynamic and adaptable prong configurations.  The integration of voltage detection and a user-selectable country further enhances its adaptability.