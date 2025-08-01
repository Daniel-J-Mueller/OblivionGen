# 10046913

**Variable Geometry Knockdown Array for Complex Object Sorting**

**Concept:** Expand the single knockdown element into a dynamically reconfigurable array, capable of selectively 'catching' or releasing objects based on size, shape, or material properties. This moves beyond simple stack separation to active object routing.

**Specifications:**

*   **Array Configuration:** Linear arrangement of independently actuatable knockdown elements ("Knockdown Cells"). Minimum of 10 cells, scalable to 100+ depending on application.
*   **Knockdown Cell Design:** Each cell incorporates a miniature pneumatic or electromagnetic actuator controlling the height/angle of the ramped surface.  Actuation range: 0-75mm vertical travel, +/- 30 degree angular adjustment.
*   **Sensing System:** Each Knockdown Cell includes:
    *   **Height Sensor:**  Ultrasonic or laser distance sensor to measure object height as it approaches.
    *   **Optical Sensor/Camera:**  For basic shape and color detection. (Optional – upgrade for advanced sorting).
*   **Control System:**
    *   **Microcontroller:**  Central processing unit responsible for coordinating cell actuation based on sensor data.
    *   **Software:** Algorithm for real-time object analysis and dynamic array configuration.  The core logic is a predictive model for object trajectory, anticipating how a rising/tilting ramp will influence its path.
*   **Conveyor Integration:** The knockdown array is integrated *above* an existing conveyor system.  Array width should match the conveyor width.
*   **Power Requirements:** 24V DC power supply.  Pneumatic actuation requires compressed air (60 PSI).
*   **Materials:** Aluminum frame, stainless steel knockdown surfaces, durable plastic for housings.

**Pseudocode (Control Algorithm – simplified):**

```
FOR each object detected:
    height = read_height_sensor()
    shape = read_shape_sensor()

    IF height > threshold_high:
        // Object is tall – raise all ramps to divert
        FOR each cell:
            set_ramp_height(cell, height_high)
    ELSE IF height < threshold_low:
        // Object is short – lower all ramps to allow passage
        FOR each cell:
            set_ramp_height(cell, height_low)
    ELSE:
        // Object is medium height - analyze shape
        IF shape == "rectangular":
            // Raise ramps on left side to guide to right
            FOR cell in range(0, array_length / 2):
                set_ramp_height(cell, height_medium)
        ELSE IF shape == "circular":
            // Raise all ramps slightly to center object
            FOR each cell:
                set_ramp_height(cell, height_low + 10mm)

    // After diversion, lower all ramps to default position
    FOR each cell:
        set_ramp_height(cell, height_default)
```

**Potential Applications:**

*   High-speed sorting of mixed-size and -shaped products in manufacturing.
*   Automated picking and packing systems.
*   Logistics and warehouse automation.
*   Quality control - diverting defective items.