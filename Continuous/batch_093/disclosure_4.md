# D636771

## Dynamic Haptic Feedback Control Pad - "MorphPad"

**Concept:** A control pad where the surface topography *actively changes* under user input, providing localized haptic feedback beyond simple vibration. The pad isn't just pressed *down*, it *morphs* to create defined shapes and textures relevant to the action being controlled.

**Specs:**

*   **Base Material:** Layered electroactive polymer (EAP) composite. Specifically, a stack of dielectric elastomer layers interspersed with conductive traces.
*   **Actuation:** Micro-electromechanical systems (MEMS) integrated into the EAP stack. Each MEMS unit controls a small area of the pad, enabling localized deformation.  Each unit is individually addressable.
*   **Resolution:** Minimum 1mm resolution for shape change across the entire pad surface. Target is 0.5mm.
*   **Deformation Range:**  0-5mm vertical displacement.
*   **Force Feedback:** Integrated force sensors within each MEMS unit measure the force applied by the user.  This data feeds back into the control system to dynamically adjust the pad’s morphology and resistance.
*   **Surface Layer:** Durable, flexible, and biocompatible coating (e.g., silicone) to provide a comfortable and sanitary user interface.
*   **Control System:** Real-time processing unit to map game/application commands to specific pad deformations.
*   **Power:** Low-voltage DC power supply.
*   **Communication:** USB-C or Bluetooth for data transfer and power.

**Operational Pseudocode:**

```
// Function: MorphPad_Update(command, force_data)

// Inputs:
//   command:  Game/Application command (e.g., "move_left", "fire", "scroll_up")
//   force_data: Array of force sensor readings (one reading per MEMS unit)

// Output: Array of MEMS actuation signals (signal for each MEMS unit)

function MorphPad_Update(command, force_data) {

    // Lookup corresponding pad morphology for the command
    morphology = LookupMorphology(command);

    // Create actuation signal array, initialized to flat state
    actuation_signals = CreateArray(MEMS_Unit_Count, 0);

    // Iterate through each MEMS unit
    for (i = 0; i < MEMS_Unit_Count; i++) {

        // Get desired height for this MEMS unit from morphology map
        desired_height = morphology[i];

        // Adjust height based on user force (force damping)
        adjustment = CalculateAdjustment(desired_height, force_data[i]);

        // Calculate actuation signal
        actuation_signals[i] = desired_height + adjustment;

        // Clamp signal to valid range
        actuation_signals[i] = Clamp(actuation_signals[i], 0, MAX_HEIGHT);
    }
    return actuation_signals;
}

//Example morphology lookups

function LookupMorphology(command){
    if(command == "move_left"){
        //create a slight ramp going from right to left across pad
        return ramp_left;
    }
    if(command == "fire"){
        //create momentary bumps
        return momentary_bumps;
    }
}

```

**Potential Applications:**

*   Gaming: Terrain simulation (feeling bumps, slopes), weapon recoil, button feedback
*   Medical: Surgical simulation, rehabilitation
*   Design/CAD: Feeling the shape of a 3D model
*   Accessibility: Providing tactile information for visually impaired users.