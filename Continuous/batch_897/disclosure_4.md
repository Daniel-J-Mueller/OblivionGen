# 9307004

## Adaptive Resource Prefetching Based on Predicted User Gaze

**Concept:** Extend prioritized transmission by proactively prefetching resources not immediately needed, but *likely* to be needed based on predicted user gaze direction and scrolling behavior. This anticipates user needs beyond what simple network prioritization can achieve, reducing perceived latency.

**Specs:**

**1. Gaze & Motion Tracking Module:**

*   **Input:** Client-side browser data: mouse movements, scroll position, touch events (if applicable), and, crucially, access to webcam feed (user permission required) for rudimentary gaze estimation.  The system should operate even *without* gaze data, falling back on historical scroll/mouse movement.
*   **Processing:**
    *   **Gaze Estimation:** Employ a lightweight (client-side) computer vision algorithm to estimate the user’s gaze direction on the screen.  Accuracy is less important than responsiveness. This can use pupil tracking or a simpler "center of focus" heuristic.
    *   **Scroll/Mouse Prediction:**  Use a Kalman filter or similar predictive algorithm to forecast future scroll position and mouse movements.
    *   **Area of Interest (AOI) Mapping:** Divide the webpage into AOIs (e.g., rectangular regions).  Assign each AOI a priority score based on its position on the page (e.g., above-the-fold elements have higher priority).
*   **Output:**  A ranked list of AOIs, with a probability score indicating the likelihood of the user’s attention falling on that area within a short timeframe (e.g., 500ms – 2s).

**2. Resource Dependency Graph:**

*   **Input:** Page source code, identifying all embedded resources (images, scripts, CSS, videos) and their dependencies.
*   **Processing:** Create a directed graph representing resource dependencies.  For example, a CSS file might be a dependency of an image.
*   **Output:**  A resource dependency graph.

**3. Prefetching Engine:**

*   **Input:** Ranked list of AOIs (from Gaze & Motion Tracking Module), Resource Dependency Graph, current network conditions (bandwidth, latency), and prioritization rules (from the existing system).
*   **Processing:**
    *   **Resource Identification:** Determine the embedded resources associated with the top-ranked AOIs.
    *   **Dependency Resolution:**  Resolve dependencies – prefetch necessary dependencies *before* the primary resource.
    *   **Prefetch Scheduling:** Schedule prefetching requests based on:
        *   Network conditions – adjust prefetch rate to avoid congestion.
        *   Prioritization rules – respect existing prioritization levels.
        *   "Time to Visibility" – prioritize resources that will become visible soonest.
    *   **Caching:** Store prefetched resources in the browser cache.
*   **Output:** Prefetch requests.

**Pseudocode (Prefetch Scheduling):**

```
function schedulePrefetch(aoiRankedList, resourceGraph, networkConditions, prioritizationRules) {
  prefetchQueue = []

  for each aoi in aoiRankedList {
    resources = getResourcesForAOI(aoi, resourceGraph)
    for each resource in resources {
      dependencies = getDependencies(resource, resourceGraph)
      prefetchQueue.append(dependencies + [resource]) //Prefetch dependencies *first*
    }
  }

  //Sort queue by time to visibility (estimated)
  prefetchQueue.sort(key=timeToVisibility)

  //Adjust prefetch rate based on network conditions
  maxPrefetchRate = calculateMaxPrefetchRate(networkConditions)
  currentPrefetchRate = 0

  for each item in prefetchQueue {
    if (currentPrefetchRate < maxPrefetchRate) {
      sendPrefetchRequest(item)
      currentPrefetchRate++
    } else {
      //Defer prefetch request
      scheduleDeferredPrefetch(item)
    }
  }
}
```

**4. Dynamic Adjustment:**

*   Monitor user gaze and scrolling behavior continuously.
*   If the user focuses on an area *without* the resources being prefetched, increase the priority of those resources for immediate download.
*   If the user *doesn't* focus on an area, reduce the priority or cancel prefetching.