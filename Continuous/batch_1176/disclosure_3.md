# 10603800

## Variable Geometry Paddle System – Bio-Inspired Morphology

**Concept:** The existing four-bar linkage primarily focuses on parallel paddle orientation during opening/closing. This expands on that by introducing *morphing* paddles – paddles capable of changing shape during the grip, inspired by the grasping mechanisms found in plant tendrils or animal tongues.

**Specs:**

*   **Paddle Material:** Utilize a shape memory alloy (SMA) mesh overlaid with a flexible, durable polymer skin. The SMA mesh forms the skeletal structure.
*   **Paddle Actuation:** Each paddle incorporates miniature SMA actuators woven into the mesh. These actuators are individually addressable.
*   **Control System:** A microcontroller (STM32 or similar) manages the SMA actuator firing sequence and intensity. This creates localized bending/curvature in the paddle surface.
*   **Sensor Integration:** Force-sensitive resistors (FSRs) are embedded within the polymer skin to detect contact force and object compliance.
*   **Linkage Modification:** Modify the existing four-bar linkage to include a slight degree of rotational freedom at the paddle connection points. This allows the paddles to ‘conform’ to the object's shape, rather than rigidly gripping.
*   **Software:** Develop a software module that translates object recognition data (from a vision system – separate component) into specific paddle actuation patterns. These patterns will optimize the grip for different object geometries and material properties.
*   **Power:** Each paddle unit will be powered through a low-voltage wiring harness integrated into the gripper arm structure.

**Pseudocode (Paddle Morphing Sequence – simplified example for spherical object):**

```
// Initialize paddle control system
init_paddles()

// Detect object and estimate its geometry
object_geometry = detect_object()

// Determine optimal paddle morphing pattern
pattern = calculate_morph_pattern(object_geometry)

// Activate paddle actuators
for each actuator in pattern:
  set_actuator_intensity(actuator, pattern[actuator])

// Monitor grip force using FSRs
grip_force = read_fsr_data()

// Adjust actuator intensity based on grip force
if grip_force < threshold:
  increase_actuator_intensity(all_actuators, delta)
else if grip_force > threshold:
  decrease_actuator_intensity(all_actuators, delta)
```

**Implementation Details:**

*   The SMA mesh density will vary across the paddle surface. Higher density at the edges for structural support, lower density in the center for flexibility.
*   The polymer skin will be chosen for its high tensile strength and resistance to abrasion.
*   A closed-loop control system using the FSR data is critical to prevent damage to the object or the gripper.
*   Consider integrating a machine learning algorithm to ‘learn’ optimal gripping patterns for different objects over time.
*   Separate housing and cooling for the SMA actuators is necessary to avoid thermal runaway.