# 9514155

## Dynamic Data ‘Sculpting’ with Haptic Feedback

**Concept:** Extend the cluster-based data presentation beyond visual representation by translating cluster density and data values into haptic feedback. Instead of *just* simplifying the visual display, provide a tactile ‘map’ of the data allowing users to ‘feel’ the density and characteristics of different areas.

**Specifications:**

*   **Data Input:** Accepts the same data streams as the existing system – sets of data items with associated values (e.g., geographic location, magnitude, time).
*   **Cluster Analysis:** Utilizes the existing clustering algorithms (or allows integration of new ones) to identify data clusters based on density and value parameters.
*   **Haptic Mapping:** Assigns haptic properties to each cluster based on its characteristics:
    *   **Density:** Cluster density maps to vibration frequency and/or amplitude. Denser clusters generate stronger/faster vibrations.
    *   **Data Value Range:**  The range of data values within a cluster maps to texture or resistance.  Higher values might correspond to rougher textures or greater resistance.  Low values to smoother textures and less resistance.
    *   **Data Value Variance:** Variance within a cluster maps to ‘noise’ or irregularity in the haptic feedback. High variance = more chaotic/irregular feedback.
*   **Haptic Device Integration:**  Interface with a wide range of haptic devices (e.g., haptic gloves, wearable arrays, force-feedback joysticks, specialized surfaces). This requires an abstraction layer to translate haptic maps into device-specific commands.
*   **Multi-Modal Presentation:**  Combine haptic feedback with the existing visual display. The visual display can highlight the currently ‘touched’ cluster or provide additional contextual information.
*   **Dynamic Adjustment:**  Allow users to dynamically adjust haptic parameters (e.g., intensity, sensitivity) to customize the experience.
*   **User Interface:**  A UI to create and store haptic profiles, allowing users to quickly switch between different data visualizations.
*   **Algorithmic Component**: Develop an algorithm to determine optimal mapping of data values to haptic feedback. This requires experimentation with different mappings to find those that are most informative and intuitive. The algorithm must account for the characteristics of the haptic device being used.

**Pseudocode for Haptic Mapping Function:**

```
function mapDataToHaptics(cluster, hapticDevice):
    density = cluster.density
    valueRange = cluster.valueRange
    valueVariance = cluster.valueVariance

    //Scale density to vibration amplitude
    vibrationAmplitude = scale(density, 0, maxDensity, 0, maxVibration)

    //Map value range to texture/resistance (using lookup table or function)
    texture = lookupTexture(valueRange) or calculateResistance(valueRange)

    //Scale variance to noise intensity
    noiseIntensity = scale(valueVariance, 0, maxVariance, 0, maxNoise)

    //Generate haptic signal
    hapticSignal = combine(vibrationAmplitude, texture, noiseIntensity)

    //Send signal to haptic device
    sendToHapticDevice(hapticSignal, hapticDevice)
end
```

**Potential Applications:**

*   Geographic data exploration (feeling population density, crime rates, etc.).
*   Financial data analysis (sensing market volatility, trade volumes).
*   Scientific visualization (exploring complex datasets through touch).
*   Accessibility tools for visually impaired users.

This system moves beyond *seeing* data to *feeling* it, creating a richer and more immersive data experience.