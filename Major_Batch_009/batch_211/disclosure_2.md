# 9576301

## Adaptive Component Blocking Based on Client-Side Resource Timing

**Concept:** Extend the core idea of detecting if a web page is loaded within a parent frame to proactively *shape* the component loading behavior based on client-side performance metrics and perceived user intent. This moves beyond simple blocking to a dynamic, client-aware loading strategy.

**Specifications:**

1.  **Client-Side Performance Monitoring:**
    *   Implement a JavaScript-based agent in the child frame to capture Resource Timing data (using the `performance.getEntries()` API) for all component requests.  Specifically, track:
        *   `fetchStart`: Time when the request was initiated.
        *   `responseEnd`: Time when the response was fully received.
        *   `entryType`:  Filter for 'fetch' entries.
    *   Calculate the `latency` as `responseEnd - fetchStart` for each component.
    *   Track historical latency data (e.g., a rolling average of the last 5-10 requests for each component) client-side.

2.  **Intent Prediction:**
    *   Utilize basic heuristics to predict user intent. Examples:
        *   **Scroll Depth:** If the user hasn't scrolled down to the section where the component would normally be rendered, delay loading it.
        *   **Mouse Hover:** If the user hasn't hovered over the area where the component is expected, delay loading.
        *   **Time on Page:** If the user has spent less than a certain time on the page (e.g., 2 seconds), delay loading.

3.  **Dynamic Component Loading Control:**
    *   The child frame JavaScript agent maintains a “loading score” for each component.  This score is calculated as follows:

        ```
        loadingScore =  (latency * weightLatency) + (intentScore * weightIntent)
        ```

        *   `latency`:  The current latency for the component.
        *   `intentScore`: A value between 0 and 1 representing the confidence of the intent prediction (higher = more likely the user wants the component).
        *   `weightLatency`:  A configurable weight for the latency component (e.g., 0.7).
        *   `weightIntent`: A configurable weight for the intent component (e.g., 0.3).

    *   Based on the `loadingScore`:
        *   **Score < Threshold_Block:** Block the component loading entirely (similar to the original patent’s approach).
        *   **Threshold_Block <= Score < Threshold_Delay:** Delay component loading by a specified amount of time (e.g., 500ms).
        *   **Score >= Threshold_Delay:** Load the component immediately.

4.  **Server-Side Coordination (Optional):**
    *   The client-side agent can periodically send aggregated latency data to the server.
    *   The server can use this data to adjust the `weightLatency` and `Threshold_Delay` values dynamically, optimizing the loading strategy for all users.
    *   This server-side adjustment could be based on A/B testing, user segments, or overall system performance.

**Pseudocode (Client-Side Agent):**

```javascript
// Component data
let componentLatencies = {};
let componentIntentScores = {};

// Configuration
const weightLatency = 0.7;
const weightIntent = 0.3;
const thresholdBlock = 0.3;
const thresholdDelay = 0.7;

// Function to calculate loading score
function calculateLoadingScore(componentName) {
  const latency = componentLatencies[componentName] || 0;
  const intentScore = componentIntentScores[componentName] || 1; // Default to 1 if no intent data
  return (latency * weightLatency) + (intentScore * weightIntent);
}

// Function to decide whether to load the component
function shouldLoadComponent(componentName) {
  const loadingScore = calculateLoadingScore(componentName);
  if (loadingScore < thresholdBlock) {
    return false; // Block
  } else if (loadingScore < thresholdDelay) {
    return 'delay'; // Delay
  } else {
    return true; // Load
  }
}

//Example Call - within the render/display logic for the component.
if (shouldLoadComponent('myComponent') === true) {
  loadComponent('myComponent');
} else if (shouldLoadComponent('myComponent') === 'delay'){
    setTimeout(() => { loadComponent('myComponent'); }, 500); // Delay loading
}
```

**Novelty:**  This extends frame detection beyond simple blocking to a *proactive*, client-aware system that adapts loading behavior based on performance and predicted user intent. It moves beyond a server-side decision to a client-side negotiation, allowing for more responsive and optimized web experiences. The system utilizes client-side performance monitoring as a key input, enabling dynamic adjustments to component loading.