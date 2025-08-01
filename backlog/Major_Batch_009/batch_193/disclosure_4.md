# 11140442

## Adaptive Content Streaming Based on Perceived User Focus

**Concept:** Dynamically adjust video stream quality *not* based on bandwidth or device capability, but on *where the user is looking*. This creates a hyper-efficient streaming experience, drastically reducing bandwidth usage without perceptible quality loss.

**Specs:**

*   **Hardware:**
    *   Eye-tracking hardware integrated into client device (TV, mobile, VR headset).  Must support reliable gaze detection.
    *   Client-side processing unit capable of real-time image processing and communication.
*   **Software – Client Side:**
    *   **Gaze Detection Module:** Captures and processes eye-tracking data to determine user's point of gaze (coordinates on the display).
    *   **Foveated Rendering Engine:**  Divides the display into zones.  The zone containing the user's gaze is designated as the "focus zone."  Peripheral zones are dynamically adjusted for lower resolution/quality.
    *   **Quality Mapping:**  Maps quality levels to zones.  Focus zone receives full quality stream.  Peripheral zones receive progressively lower quality streams (e.g., 720p, 480p, or even static images for extreme periphery).
    *   **Communication Module:**  Transmits gaze data and requested quality levels to the content server.
*   **Software – Server Side:**
    *   **Gaze Data Receiver:** Receives gaze data and quality requests from client.
    *   **Content Preparation Module:**  Maintains multiple versions of video content at varying resolutions and bitrates.  Prepares a manifest dynamically tailored to the client's gaze data.
    *   **Dynamic Manifest Generator:**  Generates a DASH or HLS manifest containing references to the appropriate content segments for each zone, based on the received gaze data.

**Pseudocode (Client Side – Simplified):**

```
loop:
    gaze_coordinates = GetGazeCoordinates()
    zone = DetermineZone(gaze_coordinates)

    if zone == "focus":
        quality = "UHD"
    elif zone == "peripheral_near":
        quality = "720p"
    elif zone == "peripheral_far":
        quality = "480p"

    RequestContentSegment(zone, quality)

    TransmitGazeData(gaze_coordinates, requested_quality)
```

**Innovation:** Existing foveated rendering focuses on VR/AR, reducing rendering load. This adapts that principle to *streaming*, shifting the optimization from the rendering pipeline to the *content delivery* pipeline. By requesting lower-quality streams for areas the user isn’t directly looking at, bandwidth usage is minimized without impacting perceived visual fidelity.  The system actively *requests* lower resolutions, rather than rendering them. This is a paradigm shift in streaming quality control.