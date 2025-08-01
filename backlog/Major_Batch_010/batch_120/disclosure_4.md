# 9494982

## Variable Density Housing with Integrated Haptic Feedback

**Concept:** Extend the foam/composite housing concept by incorporating variable density foam and embedding micro-actuators for localized haptic feedback. This creates a housing that not only protects components but also *responds* to user interaction or internal system states.

**Specs:**

*   **Materials:**
    *   Support Plate: Carbon fiber reinforced polymer matrix with a minimum elastic modulus of 70 GPa. Thickness: 0.8mm - 1.2mm.
    *   Edge Frame: Multi-density closed-cell aluminum foam. Density gradient: 0.2 g/cm³ (outermost layer) to 0.7 g/cm³ (layer contacting support plate).  Internal structure: Voronoi diagram pattern for maximized surface area and actuator integration.
    *   Actuators: Piezoelectric micro-actuators (approx. 2mm x 2mm x 1mm) embedded within the foam’s Voronoi cells. Target actuation frequency: 20Hz – 2kHz.
    *   Outer Layer: Thin, flexible polymer coating for environmental protection and tactile feel.
*   **Construction:**
    1.  Create the support plate using resin transfer molding or similar process.
    2.  Manufacture the edge frame using a foam casting/machining process. Implement a multi-density foam deposition technique to achieve the desired gradient.
    3.  Integrate piezoelectric actuators into pre-defined slots within the Voronoi structure of the edge frame *during* the foam manufacturing process. Ensure electrical connections are established.
    4.  Bond the support plate and edge frame using a thin layer of thermally conductive adhesive.
    5.  Apply the protective polymer coating to the exterior of the assembled housing.
*   **Functionality:**
    *   **Haptic Feedback:** Actuators can create localized vibrations or pressure changes on the housing surface.
        *   System Alerts: Distinct vibration patterns for low battery, incoming notifications, or error messages.
        *   Game/Application Integration: Real-time haptic feedback corresponding to on-screen events or in-game actions.
        *   User Customization: Allow users to define custom vibration patterns for specific events or applications.
    *   **Impact Absorption:** Variable density foam provides optimized impact absorption. Softer outer layers cushion initial impacts, while denser inner layers prevent damage to internal components.
    *   **Thermal Management:** Foam structure and material selection can be tuned to enhance thermal dissipation.

**Pseudocode (Haptic Control System):**

```
function onEvent(event_type, event_data):
    if event_type == "notification":
        pattern = getNotificationPattern(event_data["app"])
        playHapticPattern(pattern)
    elif event_type == "system_alert":
        pattern = getAlertPattern(event_data["severity"])
        playHapticPattern(pattern)
    elif event_type == "game_event":
        pattern = getGamePattern(event_data["action"])
        playHapticPattern(pattern)

function playHapticPattern(pattern):
    for actuator in actuators:
        actuator.frequency = pattern.frequency
        actuator.amplitude = pattern.amplitude
        actuator.duration = pattern.duration
        actuator.play()
```

**Innovation:** The use of a *variable density* foam structure allows for both optimized impact absorption *and* the creation of a dynamic, responsive housing. Embedding micro-actuators enables a new level of user interaction and information delivery through haptic feedback. This moves the housing beyond a passive protective shell and transforms it into an active component of the user experience.