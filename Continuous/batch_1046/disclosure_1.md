# 10313759

## Adaptive Fragment Prioritization & Predictive Buffering

**Concept:** The patent discusses partial fragment downloading and playback. This inspires a system which *predictively* prioritizes fragment downloads *within* a manifest, not just requesting them sequentially. It’s not enough to start playback before full download – the system should *anticipate* which parts of the next fragment(s) will be needed *soonest* and prioritize those bytes. This leverages a layered approach to video encoding.

**Specs:**

*   **Encoding Requirement:**  Video must be encoded with multiple quality layers (scalable video coding - SVC or similar).  Each layer represents a different level of detail/bandwidth.
*   **Client-Side Analysis:**
    *   **Motion Vector Analysis:**  Client analyzes downloaded frames’ motion vectors to predict areas of high motion in *future* frames.  High motion areas require more bandwidth to encode accurately.
    *   **Scene Change Detection:** Detects scene changes. Scene changes often require full frame downloads to avoid jarring visual artifacts.
    *   **Predictive Bandwidth Estimation:** Combines motion vector analysis, scene change detection, and historical bandwidth usage to *predict* the bandwidth requirement for the next N seconds of video.
*   **Manifest Modification (Client-Side):**
    *   The client dynamically re-orders requests *within* the manifest. Instead of requesting a fragment sequentially, it requests:
        *   **Base Layer First:** Always request the base layer of the next fragment immediately. This provides a minimal, but viewable, experience.
        *   **Priority Layer Request:**  Based on predictive bandwidth estimation and motion/scene analysis, request priority layers (enhancement layers) for areas predicted to have high motion or significant change.  These requests are *interleaved* with base layer requests. Example: if the next fragment has a fast-moving object, request enhancement layers for that object’s region *before* requesting enhancement layers for the static background.
        *   **Layered Byte Ranges:**  Requests are made with specific byte ranges within the enhancement layers, targeting the predicted high-motion areas.
*   **Buffering Strategy:**
    *   **Dynamic Buffer Allocation:**  Allocate more buffer space to enhancement layers requested with high priority.
    *   **Pre-fetch Enhancement Layers:** If bandwidth allows, pre-fetch enhancement layers even *before* they are requested, based on the predictive analysis.
*   **Error Handling:**
    *   If a priority layer request fails, gracefully degrade to a lower layer or a cached version of the video.
    *   Implement a fallback mechanism to request the entire fragment sequentially if predictive analysis is unreliable.

**Pseudocode (Client-Side, Fragment Request Logic):**

```
function requestFragment(manifest, fragmentIndex):
    fragment = manifest.fragments[fragmentIndex]
    baseLayer = fragment.layers[0] // Always request base layer
    request(baseLayer.url)

    priorityLayers = []
    // Analyze downloaded frames for motion vectors, scene changes, etc.
    analysisResults = analyzeFrames(downloadedFrames)

    for layer in fragment.layers[1:]:
        priority = calculatePriority(layer, analysisResults)
        if priority > threshold:
            priorityLayers.append(layer)

    //Sort layers by priority
    priorityLayers.sort(key=lambda layer: layer.priority, reverse=True)

    for layer in priorityLayers:
        //Calculate byte ranges based on motion/scene analysis
        byteRanges = calculateByteRanges(layer, analysisResults)
        for range in byteRanges:
            request(layer.url + "?" + range)

    //Update download buffer with received layers
```

**Novelty:** This goes beyond simply starting playback early. It dynamically prioritizes bytes *within* fragments based on predictive analysis, optimizing for perceived quality and reducing buffering by proactively fetching the most important data. This anticipates user visual focus.