# 9057931

**Adaptive Projection Mapping via Dynamic Display Element Control**

**Concept:** Utilize the display itself not just as an emissive surface, but as a dynamic projection surface, subtly altering the display’s pixel arrangement *during* image capture to enhance the clarity and depth information of the captured image. This goes beyond simply capturing light *through* the display; it *modifies* the display’s surface to sculpt light.

**Specs:**

*   **Display Type:** MicroLED or OLED panel with individual pixel addressing and extremely rapid refresh rates (>10kHz).  Critical: Individual pixel control *must* exceed the camera’s frame rate.
*   **Camera System:** High-speed, global shutter camera with a narrow aperture (f/2.8 or smaller) positioned behind the display.  Sensor should support high dynamic range.
*   **Control System:** Dedicated FPGA or ASIC for real-time control of the display and camera synchronization.

**Operation:**

1.  **Depth Mapping Phase:** Before each camera frame, the control system activates a sparse pattern of pixels on the display.  This pattern isn’t related to the image being displayed. Instead, it’s a calculated grid of “micro-projectors.” The brightness of each pixel in this grid is modulated according to a pre-calculated pattern. This creates subtle “shadows” and highlights on objects visible through the display.
2.  **Image Capture:**  The camera captures an image *while* the micro-projector pattern is active.
3.  **Image Processing:**  The captured image is analyzed to extract depth information based on the distortions and patterns created by the micro-projectors.  This effectively creates a simplified depth map.
4.  **Real-time Adjustment:** The micro-projector pattern is dynamically adjusted for subsequent frames based on the detected depth information. This creates a feedback loop to refine the depth map and improve accuracy.
5.  **Overlay/Augmentation:** Depth map information is used to create layered image effects, enabling rudimentary holographic-like augmentations.

**Pseudocode:**

```
//Initialization
display = new MicroLEDDisplay();
camera = new HighSpeedCamera();
depthMap = new DepthMap();

//Main Loop
while (true) {

    //1. Calculate Micro-Projector Pattern
    pattern = calculatePattern(sceneGeometry, cameraPosition);

    //2. Apply Pattern to Display
    display.setPixelPattern(pattern);

    //3. Capture Image
    image = camera.captureImage();

    //4. Extract Depth Map
    depthMap = extractDepthMap(image, pattern);

    //5. Adjust Pattern (based on depthMap, for refinement)
    refinedPattern = adjustPattern(pattern, depthMap);

    //6. Display final image (combined with augmented layers)
    display.showImage(image, depthMap);
}
```

**Novelty:**

This isn’t simply capturing light through the display. This actively *shapes* the light to create a dynamic depth map in real-time, using the display itself as a controllable light source/shaper. Existing systems use structured light *external* to the display; this integrates the structured light *into* the display. It moves beyond simple depth sensing and allows for rudimentary light field capture using only the display and camera. The rapid refresh rates and per-pixel control are critical for this to function.