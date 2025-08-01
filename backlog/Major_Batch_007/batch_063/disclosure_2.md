# 8725286

## Autonomous Inventory Rotation & Orientation System

**Concept:** Extend the mobile drive unit's capabilities beyond simple transport to include precise inventory item *orientation* within the holder, independent of the mobile unit's own rotation. This allows for automated "facing" of items for easy access or robotic arm pickup, even if the mobile unit approaches from an awkward angle.

**Specs:**

*   **Inventory Holder Modification:** Integrate a small, internal rotating platform within the inventory holder itself. This platform will support the inventory item(s). The platform’s diameter will be approximately 75% of the inventory holder’s internal dimensions.
*   **Mobile Drive Unit Additions:**
    *   **Miniature LiDAR/Depth Sensor:** Mount a downward-facing miniature LiDAR or depth sensor on the mobile drive unit *above* the docking head. Resolution: 5cm minimum. Range: 10cm minimum.
    *   **Wireless Communication Module:** Integrate a short-range wireless communication module (Bluetooth Low Energy preferred) into both the inventory holder and the mobile drive unit.
    *   **Microcontroller:** Add a microcontroller to the inventory holder to manage the rotating platform and communication.
*   **Software/Control Logic:**
    1.  **Initial Scan:** Upon docking, the mobile drive unit’s LiDAR scans the inventory item’s orientation (e.g., label facing direction).
    2.  **Orientation Calculation:** The mobile drive unit transmits the desired final orientation to the inventory holder via the wireless communication module.
    3.  **Rotation Control:** The microcontroller within the inventory holder controls a small, geared motor to rotate the internal platform, aligning the item to the desired orientation.
    4.  **Verification:** The LiDAR re-scans the item to verify correct alignment. If necessary, the rotation is adjusted.
    5.  **Transport:** Once verified, the mobile drive unit proceeds with transport to the designated location.

**Pseudocode (Inventory Holder Microcontroller):**

```
// Wireless Communication Setup

function receiveOrientation(desiredOrientation) {
    targetOrientation = desiredOrientation
    rotatePlatform()
}

function rotatePlatform() {
    currentOrientation = readPlatformOrientation()
    rotationDistance = targetOrientation - currentOrientation

    // control geared motor to rotate platform by rotationDistance
    rotateMotor(rotationDistance)

    //Wait for motor to complete rotation

    //Verify Orientation
    verifyOrientation()
}

function verifyOrientation() {
    readCurrentOrientation()
    if (currentOrientation == targetOrientation) {
        //Success
    } else {
        rotatePlatform() //recursively call until target is met.
    }
}

//initial setup/calibrations
calibratePlatform()
```

**Materials:**

*   Inventory holder: Lightweight, durable plastic or composite material.
*   Rotating platform: High-strength plastic with low friction bearings.
*   Geared Motor: Micro DC motor with encoder.
*   LiDAR/Depth Sensor: Miniature time-of-flight sensor.
*   Wireless Module: BLE module.
*   Microcontroller: Low-power ARM Cortex-M series.