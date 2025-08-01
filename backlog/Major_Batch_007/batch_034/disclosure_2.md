# 10848734

**Dynamic Foveated Encoding with Gaze-Contingent Seam Adjustment**

**Specification:**

**I. Core Concept:**

Extend the seam-based encoding concept to incorporate dynamic foveation, driven by real-time gaze tracking.  Rather than static seam definitions, the system will *actively shift* the seam location based on where the user is looking. This leverages the limitations of human visual perception – high acuity in the fovea, rapid acuity drop-off peripherally.

**II. System Components:**

1.  **Gaze Tracker:** High-resolution, low-latency eye-tracking system integrated into the VR/AR headset. Output: 2D gaze coordinates.
2.  **Projection Space Model:**  A 3D model of the virtual environment, accurately representing the projection space (cubic, cylindrical, spherical, etc.).
3.  **Seam Controller:** Software module responsible for:
    *   Calculating the seam location based on gaze direction. The seam will be positioned *just outside* the user’s foveal region.
    *   Dynamically adjusting the seam’s geometry (length, curvature) to minimize visual discontinuities.
    *   Communicating seam data to the Encoding Engine.
4.  **Encoding Engine:** Modified version of the existing encoder. Receives seam data from the Seam Controller. Modifies encoding parameters based on seam location & gaze direction, applying stronger compression to areas outside the fovea.
5.  **Decoding Engine:** Corresponding decoder to reconstruct the image based on the gaze-dependent encoding.

**III. Operational Procedure:**

1.  **Initialization:**  The system calibrates the gaze tracker and loads the Projection Space Model.  A default seam location is established.
2.  **Real-time Tracking:** The gaze tracker continuously monitors the user’s eye movements.
3.  **Seam Adjustment:** The Seam Controller calculates the new seam location based on the current gaze direction. The goal is to keep the seam at the periphery of the user's vision.
4.  **Encoding Parameter Modification:** The Encoding Engine receives the updated seam data and adjusts encoding parameters:
    *   **Quantization:**  Regions *near* the seam, but outside the fovea, receive increased quantization (more compression).
    *   **Transform Coding:**  The transform coding algorithm (e.g., DCT) prioritizes frequency components relevant to the foveal region.
    *   **Tile-based Encoding:** The image is divided into tiles. Tiles near the seam, outside the fovea, are encoded with lower resolution or simplified detail.
5.  **Encoding:** The Encoding Engine compresses the image data based on the modified parameters.
6.  **Decoding & Rendering:** The Decoding Engine reconstructs the image, and the VR/AR system renders it for display.

**IV. Pseudocode (Seam Controller):**

```pseudocode
function UpdateSeam(gazeX, gazeY, projectionModel):
  // 1. Project gaze coordinates onto the 3D projection space.
  projectedGazePoint = Project2DTo3D(gazeX, gazeY, projectionModel)

  // 2. Calculate a vector perpendicular to the user's gaze direction.
  perpendicularVector = CalculatePerpendicular(projectedGazePoint)

  // 3. Define a seam offset distance (constant or adaptive based on user preference/system load).
  seamOffset = 0.1  // Example: 0.1 meters

  // 4. Calculate the seam location by offsetting the projected gaze point along the perpendicular vector.
  seamLocation = projectedGazePoint + (perpendicularVector * seamOffset)

  // 5.  Adjust seam geometry (length, curvature) to minimize visual discontinuity (requires optimization algorithm).
  optimizedSeam = OptimizeSeamGeometry(seamLocation, projectionModel)

  return optimizedSeam
```

**V. Potential Enhancements:**

*   **Predictive Tracking:**  Use eye-tracking history to *predict* future gaze direction, allowing for pre-encoding of areas likely to come into focus.
*   **Attention Modeling:**  Incorporate attention models to predict where the user is *most likely* to look, even if their gaze isn't directly there.
*   **Multi-User Support:**  Adapt the seam location independently for each user in a multi-user VR/AR environment.
*    **Dynamic Bitrate Adjustment:** Adjust the overall bitrate based on the complexity of the scene and the user’s gaze location.