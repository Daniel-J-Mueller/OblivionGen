# D707123

## Adaptive Hinge System for Box Closure Flaps

**Concept:** Integrate a micro-actuated, shape-memory alloy (SMA) hinge system *within* the box closure flap itself, allowing the flap to dynamically adjust its opening/closing speed and resistance based on sensed weight or contents of the box. This is beyond simple friction adjustment, and moves into actively controlled motion.

**Specs:**

*   **Hinge Material:** Nickel-Titanium SMA wire woven into a flexible polymer matrix (e.g., TPU). The wire's diameter will be 0.1-0.3mm, density determined by desired force output.
*   **Hinge Placement:** Two SMA "tendons" embedded along the primary folding axis of the flap. These tendons will run the length of the hinge, spaced 5mm apart.
*   **Sensor Integration:** A miniature load cell (1mm x 1mm x 3mm) integrated *underneath* the flap, detecting the downward force exerted when closing.  This force data will be the primary input. An accelerometer could be added to detect impacts.
*   **Microcontroller:** A low-power ARM Cortex-M0 microcontroller (approx. 5mm x 5mm) housed *within* the flap's thickness. This will process sensor data and control the SMA activation.
*   **Power Source:** A thin-film flexible battery (3.7V, 50mAh) integrated into the flap, or energy harvesting via piezoelectric materials if weight/movement is sufficient. Wireless charging capability built-in.
*   **Activation Profile:** 
    *   **Low Weight (<500g):** SMA remains inactive, allowing for standard manual opening/closing.
    *   **Medium Weight (500g - 2kg):**  Microcontroller activates SMA, providing *assisted* opening/closing with dampened motion. Resistance scales with load, preventing slamming.
    *   **High Weight (>2kg):** SMA fully activates, providing smooth, controlled descent of the flap, even under substantial weight. Prevents injury/damage.
*   **Software Pseudocode:**
    ```
    // Initialize sensors, microcontroller, SMA driver
    while (true) {
      force = read_load_cell();
      if (force < 500) {
        SMA_disable();
      } else if (force >= 500 && force < 2000) {
        SMA_enable(map(force, 500, 2000, 20, 100)); // Scale activation 20-100%
      } else {
        SMA_enable(100);
      }
      delay(10ms);
    }
    ```
*   **Construction:**  The SMA tendons, load cell, microcontroller, and battery will be fully encapsulated within a protective, flexible polymer shell. The shell will be bonded to the underside of the box closure flap using a strong, flexible adhesive.
*   **Material Tolerance:** The polymer shell must be capable of withstanding bending, compression, and shear forces during normal box handling. The adhesive must maintain its bond strength over a wide temperature range.