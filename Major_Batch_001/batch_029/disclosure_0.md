# 10021458

## Dynamic Scene Graph Integration for Interactive Livestream Shopping

**Concept:** Extend the interactive overlay system to understand and dynamically respond to the *content* of the livestream, not just pre-defined coordinates. This involves creating a simplified, real-time scene graph from the video feed and linking interactive elements to objects within that scene.

**Specs:**

1.  **Scene Graph Generation Module:**
    *   Input: Live video stream.
    *   Process: Employ an object detection and segmentation model (e.g., YOLOv8, Mask R-CNN) to identify and classify objects in each frame.  Build a lightweight scene graph representing the relationships between these objects.  The graph will store:
        *   Object ID
        *   Bounding box coordinates
        *   Object class (e.g., "shoe", "shirt", "person", "table")
        *   Confidence score
        *   Relationships to other objects (e.g., "person wearing shirt")
    *   Output:  Dynamic scene graph (data structure).  Update frequency: Minimum 30 Hz.

2.  **Interactive Element Linking Interface:**
    *   Interface: Web-based GUI for content creators/producers.
    *   Functionality:
        *   Visual display of the live video stream.
        *   Overlay of the generated scene graph (optional, for debugging).
        *   Tools to select objects within the scene graph.
        *   Association of interactive overlays (e.g., buttons, hotspots, information panels) with selected objects.
        *   Configuration of interactive behavior (e.g., link to product page, trigger animation, display details).
        *   Persistence of these links in a configuration file.

3.  **Rendering Engine:**
    *   Input: Live video stream, scene graph, interactive element configurations.
    *   Process:
        *   Receive live video frames.
        *   Retrieve the current scene graph.
        *   For each interactive element:
            *   Locate the associated object in the scene graph.
            *   Transform the interactive element’s position and orientation to match the object’s in the video frame.  Implement perspective correction to handle camera movement.
            *   Render the interactive element over the video frame.
        *   Output: Composite video frame with interactive elements.

4.  **Behavior Engine:**
    *   Input: User interaction events (e.g., click, tap), scene graph, interactive element configurations.
    *   Process:
        *   Determine which interactive element was selected (raycasting from the user’s touch/mouse position onto the video frame).
        *   Trigger the associated action (e.g., open a product page, add to cart, display a popup).
        *   Potentially update the scene graph based on user interaction (e.g., highlight a selected object).

**Pseudocode (Rendering Engine):**

```
For each frame in live video stream:
    sceneGraph = GetCurrentSceneGraph()
    interactiveElements = LoadInteractiveElementConfigurations()

    For each interactiveElement in interactiveElements:
        objectID = interactiveElement.associatedObjectID
        objectData = sceneGraph.GetObject(objectID)

        If objectData is not null:
            boundingBox = objectData.boundingBox
            transformationMatrix = CalculateTransformationMatrix(boundingBox)
            transformedCoordinates = ApplyTransformation(interactiveElement.coordinates, transformationMatrix)
            RenderInteractiveElement(interactiveElement, transformedCoordinates)
    Output Composite Frame
```

**Novelty:** This moves beyond static coordinates, enabling interactive elements to *follow* objects within the live video. The system adapts to camera movements and changes in the scene without manual intervention, offering a far more immersive and engaging shopping experience. Imagine being able to tap on a dress worn by a host and immediately see product details, even as they walk around the set.