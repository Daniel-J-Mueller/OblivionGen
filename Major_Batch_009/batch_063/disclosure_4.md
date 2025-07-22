# 9465472

## Dynamic Haptic Feedback Layer Integrated with Touch Sensor

**Concept:** Integrate a microfluidic layer *behind* the touch sensor substrate (COP/COC) to create localized, dynamic haptic feedback directly correlated to on-screen interactions. This provides tactile confirmation and enhances user experience beyond simple vibrations.

**Specs:**

*   **Substrate:** Utilize the existing COP/COC substrate with specified retardation/haze/transmittance values.
*   **Microfluidic Layer:**
    *   Material: PDMS (Polydimethylsiloxane) – chosen for flexibility, biocompatibility, and ease of microfabrication.
    *   Channel Design: A dense array of microchannels (50-100 micrometers diameter) patterned directly beneath the touch sensor. Channel density adjustable based on target resolution (e.g., 1 channel per 2mm x 2mm area, up to 1 channel per 1mm x 1mm).
    *   Actuation: Individual micro-pumps (piezoelectric or micro-diaphragm) integrated with each microchannel, allowing precise control of fluid pressure.
    *   Fluid: Non-conductive, viscous fluid (silicone oil with appropriate viscosity for rapid response and noticeable tactile effect).
*   **Control System:**
    *   Touch Sensor Data Integration: Real-time processing of touch input data (location, pressure, velocity).
    *   Haptic Mapping Algorithm: Algorithm that translates touch events into specific pressure profiles for individual microchannels.  Complex interactions (e.g., sliding, multi-touch gestures) generate more complex pressure patterns.
    *   Pump Control:  Precise control of individual micro-pumps based on haptic mapping algorithm output.
*   **Layer Stack (from display outward):**
    1.  Display Module
    2.  Second OCA Layer
    3.  Touch Sensor (COP/COC with metal mesh – as per original patent)
    4.  Microfluidic Layer (PDMS with integrated pumps and channels)
    5.  Thin, protective layer (optional, to prevent fluid leakage – fluorinated polymer coating)
    6.  First OCA Layer
    7.  Cover Lens

**Pseudocode (Haptic Mapping Algorithm):**

```
function generateHapticFeedback(touchEvent):
    location = touchEvent.location
    pressure = touchEvent.pressure
    velocity = touchEvent.velocity
    gesture = detectGesture(touchEvent)

    if gesture == "tap":
        hapticProfile = createImpulseProfile(location, pressure)
    else if gesture == "slide":
        hapticProfile = createContinuousProfile(location, velocity)
    else if gesture == "multi-touch":
        hapticProfile = createComplexProfile(touchEvent.contacts) #Based on multiple contact points.
    else:
        hapticProfile = createDefaultProfile()

    return hapticProfile

function createImpulseProfile(location, pressure):
    // Define a short-duration pressure pulse localized at the touch location.
    // Pressure magnitude proportional to input pressure.
    // Duration: 50-100ms
    // Return a pressure map for specific microchannels near 'location'
    return pressureMap

function createContinuousProfile(location, velocity):
    // Define a continuous pressure profile based on sliding velocity.
    // Higher velocity = higher pressure.
    // Generate a pressure map that follows the sliding path.
    return pressureMap

// Etc.
```

**Refinements:**

*   **Thermal Integration:** Integrate micro-heaters near the microchannels to provide additional thermal feedback alongside haptic response.
*   **Variable Fluid Viscosity:** Explore techniques to dynamically adjust fluid viscosity within the microchannels using external stimuli (e.g., electric field). This allows for fine-tuning of haptic sensation.
*   **Biofeedback Integration**: Add sensors which can detect skin conductance and pulse rate, so that the feedback is also affected by the users state.