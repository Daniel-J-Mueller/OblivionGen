# 12102217

## Modular Handle System with Integrated Sensory Feedback

**Concept:** Expand the convertible bag’s handle functionality by creating a modular handle system with integrated sensory feedback. This allows the bag to 'communicate' information to the user through haptic and potentially visual signals embedded within the handles.

**Specs:**

1.  **Handle Modules:** The core of the system consists of interchangeable handle modules. These modules are approximately 15cm long and 3cm in diameter, designed to connect securely to anchor points on the bag body. Connection mechanism: Magnetic bayonet mount with locking feature.

2.  **Base Module:** The base module is a standard handle section providing basic carrying functionality. Materials: High-strength polymer composite, wrapped in ergonomic silicone grip. Contains the magnetic connector and primary structural support.

3.  **Haptic Feedback Module:** This module integrates miniature linear resonant actuators (LRAs) within the handle. Control is handled by a Bluetooth Low Energy (BLE) microcontroller.
    *   Actuation Pattern Library: Pre-programmed patterns for common notifications (incoming call, low battery, navigation cues).
    *   Customizable Patterns: User-definable patterns via a companion mobile app.
    *   Power Source: Rechargeable lithium-ion battery within the module (approx. 2-hour charge time, 8-hour runtime).
    *   Microcontroller: Nordic nRF52840 BLE SoC.

4.  **Lighting Module:** This module integrates addressable RGB LEDs into the handle.
    *   LED Density: 16 LEDs per module.
    *   Color Customization: Full RGB spectrum via mobile app.
    *   Dynamic Lighting Effects: Pre-programmed effects (pulse, strobe, flow) and customizable animations.
    *   Power Source: Shares battery with Haptic Feedback Module when paired.

5.  **Sensor Module:** Integrates an inertial measurement unit (IMU – accelerometer, gyroscope) and a proximity sensor.
    *   Gesture Recognition: Detects specific handle gestures (tap, shake, rotation) for controlling connected devices or accessing bag functions.
    *   Proximity Detection: Activates lighting or haptic feedback when the bag is approached.

6.  **Modular Connection Interface:** The anchor points on the bag body will be redesigned to accommodate a standardized circular rail system.
    *   Rail Material: Lightweight aluminum alloy.
    *   Module Attachment: Magnetic locking mechanism for secure attachment and easy module swapping.
    *   Data Communication: Wireless data transfer (BLE) between the modules and a central hub within the bag.

7. **Central Hub:** Embedded within the bag's lining, handles communication between modules and external devices.
    *   Microcontroller: ESP32
    *   Connectivity: Bluetooth 5.0, Wi-Fi

**Pseudocode (Module Communication):**

```
// Module (Haptic, Lighting, Sensor)
function receiveData(data){
    if (data.type == "notification"){
        triggerHapticPattern(data.pattern)
        setLEDColor(data.color)
    }
    else if (data.type == "gesture"){
        sendGestureData(data.gesture)
    }
}
function sendGestureData(gesture){
    // Transmit gesture data to Central Hub via BLE
}

// Central Hub
function receiveModuleData(moduleID, data){
    // Process data from module
    // Route data to appropriate external device (e.g., phone)
}
function sendCommandToModule(moduleID, command, data){
    // Transmit command to module via BLE
}

```

**User Interface (Mobile App):**

*   Module Configuration: Customize module functions and settings.
*   Haptic Pattern Editor: Create custom haptic patterns.
*   Lighting Effects Designer: Design custom lighting effects.
*   Gesture Mapping: Assign gestures to specific actions.
*   Battery Level Monitoring: Track battery levels of individual modules.