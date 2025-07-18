# 10694189

## Dynamic Reference Frame Prioritization & Cascading

**Concept:** Extend bandwidth adaptation not just to *how* a reference frame is accessed (window size), but *which* reference frame is prioritized, and cascade bandwidth usage across multiple frames. 

**Specification:**

**I. System Components:**

*   **Bandwidth Monitor:** (As in patent) Tracks available bandwidth to external memory.
*   **Reference Frame Queue:** A prioritized queue holding N recently used/potentially useful reference frames. Priority is dynamically adjusted (see II).
*   **Frame Cost Estimator:**  Calculates a ‘cost’ for accessing each frame in the Reference Frame Queue. Cost is a function of:
    *   Frame size (resolution).
    *   Temporal distance from current frame (further = less priority, potentially lower quality).
    *   Motion vector density (higher density = more data required).
    *   Prediction mode dependency (some modes *require* specific reference frames).
*   **Cascading Buffer:** A small, fast buffer (SRAM on-chip) to hold *portions* of high-priority reference frames. Allows rapid access to frequently used sections.
*   **Adaptive Resolution Scaler:**  Scales down the resolution of lower-priority reference frames *before* storing them. This reduces bandwidth cost, but introduces some quality loss.
*   **Prediction Mode Aware Selector:**  Selects inter-prediction modes *knowing* the available reference frame data.  Disables modes that rely on unavailable/low-quality frames.

**II. Operational Flow:**

1.  **Frame Arrival:** A new frame is encoded.
2.  **Cost Estimation:** The Frame Cost Estimator calculates the cost for potential reference frames based on the current frame.
3.  **Queue Management:**
    *   Highest cost frames are placed at the rear of the Reference Frame Queue.
    *   Lowest cost frames move towards the front.
    *   If the queue is full, the highest cost frame is evicted (or downgraded - see IV).
4.  **Bandwidth Allocation:** The system allocates bandwidth based on the priority of the reference frames.
5.  **Access Strategy:**
    *   Highest priority frames are loaded entirely into memory.
    *   Frequently accessed portions of these frames are cached in the Cascading Buffer.
    *   Lower priority frames are either:
        *   Loaded at reduced resolution.
        *   Accessed only for key sections (using a region-of-interest approach).
        *   Skipped entirely, with prediction mode selection adjusted accordingly.
6.  **Prediction & Encoding:** Inter-prediction is performed using the available reference frame data.

**III. Pseudocode (Bandwidth Allocation):**

```
// Variables:
AvailableBandwidth = // From Bandwidth Monitor
ReferenceFrameQueue = // List of Reference Frames (sorted by priority)
FrameCosts = // List of frame costs (corresponding to Queue)

TotalAllocatedBandwidth = 0
For Each Frame in ReferenceFrameQueue:
    If TotalAllocatedBandwidth < AvailableBandwidth:
        If FrameCost[Frame] < BandwidthThreshold: //Adjust Threshold Based on current load
            Allocate Full Frame Bandwidth
            TotalAllocatedBandwidth += FrameBandwidth
        Else:
            //Scale down resolution to fit within bandwidth
            ScaledFrameBandwidth = ScaleResolution(Frame, AvailableBandwidth - TotalAllocatedBandwidth)
            If ScaledFrameBandwidth > 0:
                Allocate ScaledFrameBandwidth
                TotalAllocatedBandwidth += ScaledFrameBandwidth
            Else:
                //Frame cannot fit, skip. Update inter-prediction mode selector
                DisablePredictionModesRequiringFrame(Frame)
    Else:
        DisablePredictionModesRequiringFrame(Frame)
        Break
```

**IV. Dynamic Resolution Scaling:**

Employ a multi-level resolution scaling scheme.  Instead of simply reducing resolution, use a hierarchy of downscaled versions (e.g., 100%, 75%, 50%, 25%). This allows for a trade-off between bandwidth and quality. The system can dynamically switch between these levels based on the available bandwidth and the importance of the reference frame.

**V. Inter-Prediction Mode Adaptation:**

The system maintains a “compatibility matrix” linking inter-prediction modes to required reference frame data. If a reference frame is unavailable or of low quality, the system automatically disables or downgrades the corresponding prediction modes.