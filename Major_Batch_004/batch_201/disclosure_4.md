# 9811188

## Dynamic Black Masking & Perceived Depth

**Concept:** Enhance the visual experience by dynamically adjusting black masks *within* the display stack to create a sense of depth and focus attention. This builds on the existing black mask ideas in the patent, but moves beyond static placement.

**Specs:**

*   **Display Stack Component:** Addition of a micro-LED array positioned *between* the second OCA layer and the front light component. This array is extremely high density – aiming for >1000 PPI.  Individual LEDs are RGB, and capable of very low brightness/off states.
*   **Control System:**  A dedicated image processing unit (IPU) integrated into the device, responsible for analyzing content and generating dynamic black mask patterns. This IPU utilizes a machine learning model trained to identify salient features and areas of interest in displayed content (e.g., faces, text, objects).
*   **Black Mask Pattern Generation:**
    *   The IPU generates a “depth map” based on the identified features in the content.  Closer features have more intense (darker) masking around them, simulating a narrow depth of field.  Distant features have lighter or no masking.
    *   Masking isn't limited to the edges of the display; the micro-LED array creates *internal* dynamic masks, blurring or darkening areas of the image in real-time to guide the viewer’s eye.
    *   Masking intensity and shape adapt to viewing angle via an integrated IMU (Inertial Measurement Unit).  This maintains the illusion of depth regardless of how the device is held.
*   **Layer Integration:** The micro-LED array is encapsulated in a transparent material with refractive index matching the surrounding OCA layers to minimize light scattering.  Electrical connections are routed via flexible printed circuits integrated into the second OCA layer.
*   **Software API:**  SDK (Software Development Kit) provided to app developers allowing them to customize dynamic masking behavior or integrate their own depth estimation algorithms.

**Pseudocode (Simplified IPU Logic):**

```
function generate_dynamic_mask(frame, depth_estimation_model) {
  depth_map = depth_estimation_model.estimate_depth(frame)
  mask_intensity = calculate_mask_intensity(depth_map)
  dynamic_mask = apply_mask_intensity(mask_intensity, frame)
  return dynamic_mask
}

function calculate_mask_intensity(depth_map) {
  // Normalize depth values to a range of 0-1
  normalized_depth = normalize(depth_map)

  // Apply a sigmoid function to create a smooth intensity curve
  intensity = sigmoid(normalized_depth)

  return intensity
}

function sigmoid(x) {
  return 1 / (1 + exp(-x))
}

function apply_mask_intensity(intensity, frame) {
  //Apply masking via LED array.
  //Scale intensity to LED brightness.
  return masked_frame
}

```

**Potential Benefits:**

*   Increased visual immersion.
*   Improved readability, particularly for text.
*   Enhanced focus on key content elements.
*   Potential for AR/VR applications where depth perception is critical.