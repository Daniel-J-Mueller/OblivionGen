# 9948740

## Adaptive Fragment Reconstruction

**Concept:** The patent focuses on caching and translating media fragments based on different streaming protocols. This inspires a system for *reconstructing* fragments on the fly, adapting to network conditions and client capabilities *beyond* just protocol translation. Instead of serving pre-cut fragments, the edge server dynamically assembles fragments from smaller, more granular media units.

**Specifications:**

**1. Granular Media Units (GMUs):**
   *   Media content is ingested and segmented into GMUs – extremely short duration video/audio clips (e.g., 10-50ms).
   *   GMUs are encoded with multiple quality levels (bitrates, resolutions) and stored in the edge server's cache.
   *   GMUs are indexed by content ID, time offset within the original media, and quality level.

**2. Adaptive Fragment Assembly Engine:**
   *   Resides on the edge server.
   *   Receives a request for a media fragment (identified by time range).
   *   Analyzes client bandwidth (using a probe or historical data) and device capabilities (resolution, codec support).
   *   Selects the optimal combination of GMUs to fulfill the fragment request, considering:
        *   Bandwidth constraints – prioritizes lower bitrate GMUs if bandwidth is limited.
        *   Device capabilities – selects GMUs with supported codecs and resolutions.
        *   Seamless playback – ensures GMUs are contiguous in time and create a smooth transition.
   *   Concatenates the selected GMUs into a new fragment.
   *   Packages the new fragment according to the requested streaming protocol.

**3. Caching Strategy:**
   *   Cache GMUs, *not* pre-cut fragments. This significantly reduces cache storage requirements and increases flexibility.
   *   Implement a Least Recently Used (LRU) or similar caching algorithm for GMUs.
   *   Pre-fetch GMUs proactively based on predicted viewer demand (e.g., based on trending content).

**4. Manifest Generation:**
   *   The manifest file does *not* list pre-cut fragments. Instead, it lists the available GMUs and their attributes (time offset, bitrate, resolution).
   *   The client requests a time range, and the edge server dynamically assembles the corresponding GMUs based on the client's capabilities and network conditions.

**Pseudocode (Edge Server Fragment Assembly):**

```
function assembleFragment(request):
  clientBandwidth = getClientBandwidth(request)
  clientCapabilities = getClientCapabilities(request)
  startTime = request.startTime
  endTime = request.endTime

  gmUs = []
  currentTime = startTime

  while currentTime < endTime:
    bestGMU = findBestGMU(currentTime, clientBandwidth, clientCapabilities)

    if bestGMU is null:
      //Handle missing GMU (e.g., use lower quality)
      break

    gmUs.append(bestGMU)
    currentTime += bestGMU.duration

  assembledFragment = concatenate(gmUs)
  packagedFragment = packageFragment(assembledFragment, request.protocol)

  return packagedFragment
```

**Innovations:**

*   **Beyond Protocol Translation:** Adapts to *dynamic* network conditions and client capabilities *at the fragment level*.
*   **Reduced Cache Footprint:** Caching GMUs is more efficient than caching pre-cut fragments.
*   **Increased Flexibility:** Supports a wider range of client devices and network conditions.
*   **Seamless Playback:** Algorithm prioritizes contiguous GMUs for smooth transitions.