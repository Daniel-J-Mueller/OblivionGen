# 9514333

## Secure Peripheral Data Masking

**Concept:** Expand obscuring sensitive data during remote support sessions beyond just screen content. Extend the masking to *peripheral* input data streams – mouse movements, keyboard strokes, touch input – before they reach the support agent. This builds upon the patent’s focus on obscuring screen content, adding a layer of protection for dynamic input.

**Specification:**

**1. Hardware Components:**

*   **Secure Input Interception Module (SIIM):** A low-level driver/firmware module operating between the input device (keyboard, mouse, touchscreen) and the operating system. This module is responsible for intercepting raw input data.
*   **Data Obfuscation Engine (DOE):** A software component residing within the SIIM or as a separate kernel module. Responsible for applying obfuscation algorithms to raw input data.
*   **Remote Support Application Integration:** Modifications to the remote support application to signal the SIIM/DOE when a remote session is active and to define obfuscation levels.

**2. Software Logic (Pseudocode):**

```pseudocode
// Remote Support Application initiates session
ON RemoteSessionStart(obfuscationLevel) {
    SIIM.setActive(TRUE)
    SIIM.setObfuscationLevel(obfuscationLevel)
}

// SIIM intercepts input
ON InputEvent(eventType, data) {
    IF (SIIM.isActive()) {
        obfuscatedData = DOE.obfuscate(eventType, data, SIIM.getObfuscationLevel())
        OS.processInput(eventType, obfuscatedData) // Send obfuscated data to OS
    } ELSE {
        OS.processInput(eventType, data) // Send raw data to OS
    }
}

// DOE Obfuscation Algorithms (examples)
FUNCTION DOE.obfuscate(eventType, data, level) {
    IF (level == 0) { // No obfuscation
        RETURN data
    }
    IF (eventType == "mouse_move") {
        // Option 1: Quantize mouse movements (reduce precision)
        quantizedX = round(data.x / quantizationFactor) * quantizationFactor
        quantizedY = round(data.y / quantizationFactor) * quantizationFactor
        RETURN {x: quantizedX, y: quantizedY}
        // Option 2: Introduce random jitter (small, controlled variations)
        jitterX = random(-jitterRange, jitterRange)
        jitterY = random(-jitterRange, jitterRange)
        RETURN {x: data.x + jitterX, y: data.y + jitterY}
    }
    IF (eventType == "keyboard_press") {
        // Option 1: Delay key presses (introduce latency)
        delayedChar = delay(data.char, delayAmount)
        RETURN delayedChar
        // Option 2: Swap adjacent characters (mild character scrambling)
        swappedChar = swap(data.char, nextChar)
        RETURN swappedChar
    }
    RETURN data // Default: No obfuscation
}
```

**3. Obfuscation Levels:**

*   **Level 0 (None):** Raw input data is sent to the support agent.
*   **Level 1 (Low):** Moderate quantization of mouse movements, slight delays in key presses.
*   **Level 2 (Medium):** Higher quantization, more significant delays, basic character swapping.
*   **Level 3 (High):** Extreme quantization, substantial delays, advanced character scrambling, simulated ‘phantom’ inputs.

**4. Implementation Notes:**

*   The SIIM must be designed with security in mind to prevent tampering or bypass.
*   Performance is critical; obfuscation algorithms must be efficient to avoid noticeable lag.
*   User configuration options to adjust obfuscation levels based on sensitivity of the task.
*   Compatibility with a wide range of input devices and operating systems.