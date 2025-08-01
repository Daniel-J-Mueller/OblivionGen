# 8113855

## Modular, Self-Configuring Power Strip Adapter

**Concept:** A power adapter building block system allowing users to create custom power configurations based on regional plug types *and* device power needs. This goes beyond simple plug swapping to include integrated power management and diagnostics.

**Specs:**

*   **Core Module:** Cylindrical housing (~7cm diameter, ~5cm length). Contains AC-to-DC converter (universal input 100-240V, output configurable - see below), microcontroller, wireless communication module (Bluetooth/WiFi), and status LEDs. Houses a retractable universal plug (Type A, B, C, G, I – selectable digitally).
*   **Expansion Modules:**
    *   **Plug Modules:** Smaller cylindrical housings (~3cm diameter, ~3cm length) with a single retractable plug type (A, B, C, G, I). Connects magnetically to the Core Module.
    *   **Output Modules:**  Provide various output types:
        *   USB-A (standard, fast charging)
        *   USB-C (Power Delivery – configurable voltage/current)
        *   Wireless Charging Pad (Qi standard)
        *   DC Barrel Jack (multiple voltages selectable)
        *   AC Outlet (limited to Core Module's max wattage)
    *   **Sensor Modules:** (Optional)
        *   Power Meter (measures voltage, current, power consumption)
        *   Temperature Sensor (monitors module temperature)
*   **Connectivity:**
    *   Mobile App (iOS/Android) – Used for module configuration, power monitoring, scheduling, and firmware updates.
    *   API – Allows integration with smart home ecosystems (e.g., IFTTT, Alexa, Google Assistant).
*   **Power Management:**
    *   Individual Output Control – Users can enable/disable outputs remotely.
    *   Power Limiting –  Users can set a maximum power draw for the entire adapter or individual outputs.
    *   Overload Protection – Automatic shutdown in case of overload.
*   **Physical Connection:**
    *   Magnetic Connector – Modules connect magnetically to the Core Module, providing a secure and easy connection.
    *   Conductive Pins –  Ensure reliable power transfer between modules.
*   **Retraction Mechanism:**
    *   Motorized retraction of both core and expansion module plugs via app control.
    *   Manual override switch for emergency use.

**Pseudocode (Module Detection & Configuration):**

```
// Core Module Initialization
function initialize() {
    scanForModules();
    connectToMobileApp();
}

function scanForModules() {
    // Broadcast a signal to detect nearby modules
    // Modules respond with their type and ID
    moduleList = [];
    // Loop through responses and add modules to the list
    for each response in detectedResponses {
        module = createModuleObject(response.type, response.id);
        moduleList.append(module);
    }
}

function createModuleObject(type, id) {
    switch type {
        case "PLUG":
            module = new PlugModule(id);
        case "USB-A":
            module = new USBAOutputModule(id);
        case "USB-C":
            module = new USBCOutputModule(id);
        case "WIRELESS_CHARGER":
            module = new WirelessChargerOutputModule(id);
        default:
            module = new UnknownModule(id);
    }
    return module;
}

function connectToMobileApp() {
    // Establish Bluetooth/WiFi connection with the mobile app
    // Allow user to configure modules and set power limits
}

// Example Module Class
class USBAOutputModule {
    constructor(id) {
        this.id = id;
        this.type = "USB-A";
        this.voltage = 5V;
        this.currentLimit = 2.4A;
    }
}
```

**Innovation:** This system moves beyond a static adapter to a dynamic power management solution. Users can customize their power configurations on the fly, adapt to different regional standards, and monitor power consumption. The modular design allows for easy expansion and upgrades.