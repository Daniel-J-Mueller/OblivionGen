# 11770284

**Adaptive Constellation Morphing for Per-Carrier Cancellation**

**Concept:** Dynamically alter the constellation of each carrier within the multi-carrier signal *during* the cancellation pulse scaling process, not just pre-transmission, to optimize EVM and PAPR simultaneously. This moves beyond static target EVM values linked to constellation choice, enabling fine-grained control and greater signal resilience.

**Specs:**

*   **Hardware:**
    *   Multi-carrier Signal Generator (as in patent).
    *   Digital Signal Processing (DSP) unit with high throughput and low latency.
    *   Fast Fourier Transform (FFT) and Inverse FFT (IFFT) modules.
    *   Analog-to-Digital Converter (ADC) and Digital-to-Analog Converter (DAC) for feedback control.
    *   High-speed memory for storing constellation maps and scaling factors.

*   **Software/Algorithms:**
    1.  **Initial Setup:** Identify target PAPR and initial target EVM range for each carrier. This initial EVM range acts as a starting point for the dynamic adjustments.
    2.  **Constellation Map Library:** Pre-define a library of constellations (QPSK, 16QAM, 64QAM, etc.) and *interpolations* between them.  Crucially, include "fractal" constellation variations – slight, randomized deviations from standard shapes.
    3.  **Cancellation Pulse Scaling (Modified):**
        *   Begin standard cancellation scaling as per the patent.
        *   **EVM Monitoring:** Continuously monitor the EVM of *each* carrier in real-time *during* the scaling process.
        *   **Constellation Morphing Trigger:**  If a carrier's EVM deviates significantly from its target, trigger a small “morph” to a slightly different constellation. This morph is not a full switch, but a gradual blend of the current constellation with a neighboring one (or fractal variation) in the library.
        *   **Morph Parameter Control:** The ‘amount’ of morph is controlled by the magnitude of the EVM deviation. Larger deviations = larger morphs, up to a maximum allowable change per iteration.
        *   **PAPR Constraint:** After each morph, re-evaluate the overall PAPR. If the PAPR exceeds the target, *slightly* reduce the scaling factor for that carrier *and* morph back towards the original constellation.
        *   **Iterative Adjustment:** Repeat steps 3-6 until all carriers meet their EVM targets *and* the overall PAPR is within bounds.
    4.  **Feedback Loop:** Implement a feedback loop using the ADC/DAC to monitor the transmitted signal and refine the morphing algorithm over time, improving performance and adapting to changing channel conditions.

*   **Pseudocode (Morphing Function):**

```
function morphConstellation(currentConstellation, evmDeviation, maxMorphAmount):
    // Lookup neighboring constellations in the library
    neighborConstellations = findNeighbors(currentConstellation)

    // Calculate morph factor based on EVM deviation
    morphFactor = constrain(evmDeviation * gainFactor, 0, maxMorphAmount)

    // Blend current and neighboring constellations
    newConstellation = (1 - morphFactor) * currentConstellation + morphFactor * randomNeighbor(neighborConstellations)

    return newConstellation
```

*   **Data Structures:**
    *   `CarrierData`:  Structure containing: `currentConstellation`, `targetEVM`, `scalingFactor`, `evmDeviation`.
    *   `ConstellationLibrary`: Database of pre-defined and interpolated constellations.

**Rationale:** This approach moves beyond static EVM targets and allows the system to dynamically adapt to the specific characteristics of each carrier and the overall signal. Fractal constellation variations introduce additional degrees of freedom and can improve robustness against noise and interference. The feedback loop enables continuous optimization and adaptation to changing channel conditions.