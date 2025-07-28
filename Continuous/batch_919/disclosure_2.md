# 9123272

## Dynamic Occlusion Mapping for Volumetric Displays

**Concept:** Extend the shadow-based light source determination to enable realistic lighting of *volumetric* displays – displays which project images *in* three-dimensional space, rather than on a two-dimensional surface (think holographic projections, or light-field displays). Instead of just rendering on a screen, we're lighting the volume *itself*.

**Specifications:**

**1. Sensor Suite:**

*   **Multi-Spectral Sensor Array:**  Array of 8x8 micro-sensors, each capable of detecting ambient light across the visible spectrum + near-infrared.  Resolution: 200x200 pixels total (400x400 effective, with interpolation).  Placement: Bezel of the display device.
*   **Time-of-Flight (ToF) Sensor:** Integrated ToF sensor (resolution 640x480) for depth mapping of the surrounding environment. Range: 0.1m - 5m. Used for occlusion volume reconstruction.
*   **Inertial Measurement Unit (IMU):**  9-axis IMU (accelerometer, gyroscope, magnetometer) for precise orientation tracking. Update rate: 200Hz.

**2. Occlusion Volume Reconstruction:**

*   **Algorithm:**  Real-time point cloud generation from ToF data. Filtering to identify static occluding objects (furniture, hands, etc.). Dynamic occluding objects (hands, moving objects) tracked via frame-to-frame motion estimation.
*   **Data Structure:** Octree representation of the occlusion volume.  Each node stores the minimum and maximum bounds of the occluded space.  Nodes are dynamically subdivided based on occlusion density.
*   **Calibration:** Automated calibration procedure to map the sensor array and ToF sensor to the volumetric display space.

**3. Light Source Estimation:**

*   **Shadow Edge Detection:** Utilize image processing algorithms (Sobel, Canny edge detection) on the multi-spectral sensor array data to identify shadow edges.
*   **Ray Tracing (Simplified):** For each shadow edge, cast multiple rays backward from the sensor.  Rays intersect with the reconstructed occlusion volume.  The intersection point provides a constraint on the light source direction.
*   **Optimization:** Employ a non-linear optimization algorithm (e.g., Levenberg-Marquardt) to estimate the light source position and direction that best fits all observed shadow edges and occlusion data.
*   **Multi-Light Source Support:** Algorithm extended to support multiple light sources.

**4. Volumetric Lighting and Shading:**

*   **Volumetric Rendering:** Utilize a voxel-based volumetric rendering engine.  The volumetric display space is divided into voxels.
*   **Light Transport Simulation:** Simulate light transport through the voxel grid using a simplified ray tracing algorithm.  Account for absorption, scattering, and reflection.
*   **Shadow Mapping:** Generate a shadow map from the estimated light source position and direction.  Apply shadow mapping to the voxel rendering process.
*   **Dynamic Lighting Effects:** Implement dynamic lighting effects (e.g., specular highlights, ambient occlusion) to enhance realism.

**5. Pseudocode (Light Source Estimation):**

```
function estimateLightSource(shadowEdges, occlusionVolume):
  // shadowEdges: List of shadow edge pixel coordinates
  // occlusionVolume: Octree representing occluded space

  initialGuess = // Some reasonable initial light source position/direction
  
  function costFunction(lightSourcePosition, lightSourceDirection):
    totalCost = 0
    for each edge in shadowEdges:
      // Trace a ray from the edge pixel backwards through the scene
      intersectionPoint = rayIntersectOcclusionVolume(ray, occlusionVolume)

      if intersectionPoint:
        // Calculate the distance between the intersection point and the
        // predicted light source position.
        distance = distance(lightSourcePosition, intersectionPoint)
        totalCost += distance * distance  // Minimize squared distance
    return totalCost

  // Use optimization algorithm to minimize costFunction
  optimizedLightSourcePosition, optimizedLightSourceDirection =
    optimize(costFunction, initialGuess)

  return optimizedLightSourcePosition, optimizedLightSourceDirection
```

**6. Hardware Considerations:**

*   Dedicated co-processor (GPU or FPGA) for real-time processing of sensor data and volumetric rendering.
*   High refresh rate volumetric display for smooth rendering of dynamic lighting effects.
*   Low-latency communication interface between the sensor suite, co-processor, and display.