# 10659512

## Dynamic Segment Composition via Predictive Pre-fetching & Modular Encoding

**Concept:** Shift from pre-defined segment lists to dynamically composed segments based on predicted client needs and a modular encoding system. Instead of a fixed segment duration, the system assembles segments *on-the-fly* from smaller, independently encoded “micro-segments”.

**Motivation:** The existing patent focuses on *selecting* from pre-existing bitrates. This proposes a system where the bitrate *itself* is a dynamically assembled property of the content, reducing overhead and optimizing for real-time conditions. It is about building, not choosing.

**System Specs:**

*   **Micro-Segment Encoding:** Content is initially encoded into extremely short, independent “micro-segments” (e.g., 100ms - 200ms duration).  Each micro-segment is encoded *multiple* times, at a range of bitrates and potentially different codecs (e.g., AV1, H.264). These constitute a “micro-segment library”. Metadata associated with each micro-segment includes:
    *   Bitrate
    *   Codec
    *   Perceptual Quality Score (VMAF, PSNR)
    *   Computational Complexity (encoding/decoding cost)
*   **Predictive Prefetching Engine:**  This engine, running at edge locations, analyzes client-specific data (bandwidth, device capabilities, historical viewing patterns) to *predict* the optimal bitrate/codec combination for the *next* few seconds of content.  The prediction considers not just current conditions, but *predicted* changes (e.g., anticipating a drop in bandwidth based on time-of-day usage patterns).
*   **Dynamic Segment Assembler:**  Based on the predictions from the Prefetching Engine, this component assembles the required micro-segments into larger segments *on-demand*. The assembler prioritizes micro-segments that meet the predicted bitrate/codec needs while minimizing computational cost.
*   **Client-Side Buffering Enhancement:** Clients maintain a dynamic buffer of pre-fetched micro-segments. This allows for seamless switching between bitrates/codecs without noticeable interruption.
*   **Historical Feedback Loop:** The system continuously monitors client-side buffering events, playback quality, and bandwidth usage. This data is fed back to the Predictive Prefetching Engine to refine its predictions and improve overall performance.

**Pseudocode (Dynamic Segment Assembler):**

```
function assembleSegment(requestTime, duration, predictedBitrateProfile):
    segmentData = []
    currentTime = requestTime
    while currentTime < requestTime + duration:
        bestMicroSegment = findBestMicroSegment(currentTime, predictedBitrateProfile)
        segmentData.append(bestMicroSegment)
        currentTime += bestMicroSegment.duration

    return segmentData

function findBestMicroSegment(currentTime, bitrateProfile):
    microSegments = queryMicroSegmentLibrary(currentTime)
    filteredSegments = filterByBitrate(microSegments, bitrateProfile)
    bestSegment = selectLowestComplexitySegment(filteredSegments) #Prioritizes efficient decoding
    return bestSegment
```

**Novelty:** This approach moves beyond adapting from pre-built segments to *building* segments in real-time. The modular encoding and predictive prefetching create a highly adaptive and efficient streaming experience. It leverages the potential of edge computing to deliver personalized content streams tailored to individual client needs and network conditions.