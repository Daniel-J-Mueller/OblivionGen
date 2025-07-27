# 11666823

## Dynamic Focal Point Streaming

**Concept:** Extend the dynamic range conversion concept to prioritize and enhance video quality *around* a user-defined focal point in both local and remote streams. This allows for a variable bitrate/resolution system where the area the user is actively looking at (or intends to look at) receives the highest fidelity, while peripheral areas are compressed more aggressively.

**Specifications:**

**1. Input Sources:**

*   **Primary Video:** Game video (HDR) – standard input.
*   **Secondary Video:** Camera video (SDR) – standard input.
*   **Gaze Tracking/Intent Prediction:** Integrate with eye-tracking hardware (VR/AR headsets, specialized cameras) *or* utilize AI-powered intent prediction based on game actions (where the player is aiming, moving toward, etc.).  Output: X,Y coordinates representing the focal point on the rendered game scene and camera view.

**2. Processing Pipeline:**

*   **Focal Point Mapping:** Map the X,Y coordinates from the gaze/intent tracking system onto both the game and camera video feeds.
*   **Variable Resolution/Bitrate Encoding:**  Implement a concentric zone system around the focal point. 
    *   **Zone 0 (Focal Point):**  Maintain maximum resolution and bitrate (HDR for game video, upscaled/enhanced SDR for camera video).
    *   **Zone 1 (Inner Ring):**  High resolution/bitrate, slight compression.
    *   **Zone 2 (Middle Ring):** Moderate resolution/bitrate, noticeable compression.
    *   **Zone 3 (Outer Ring):**  Low resolution/bitrate, significant compression.
*   **Dynamic Range Conversion:** As described in the source patent, convert HDR to SDR as needed for remote transmission. Apply this *after* variable resolution/bitrate encoding.
*   **Remote Stream Encoding:** Encode the variable-resolution/bitrate HDR/SDR streams for network transmission.
*   **Local Stream Encoding:** Encode the variable-resolution/bitrate HDR/SDR streams for local display.

**3. Output Streams:**

*   **Remote Stream:** Variable-resolution/bitrate HDR/SDR stream for remote viewers.
*   **Local Stream:** Variable-resolution/bitrate HDR/SDR stream for local display.  Local display should be capable of rendering variable resolution sections within a single frame.

**4. Pseudocode (Simplified):**

```
// Per Frame
GetFocalPoint(focalX, focalY)

// For each video source (game, camera)
For each pixel in video frame
    distance = calculateDistance(pixelX, pixelY, focalX, focalY)

    If distance < radiusZone1:
        quality = High
    Else If distance < radiusZone2:
        quality = Medium
    Else If distance < radiusZone3:
        quality = Low
    Else:
        quality = VeryLow

    pixel = applyQualitySettings(pixel, quality) // Compression/Upscaling/Bitrate

    If isRemoteStream:
        If pixel is HDR:
            convertHDRtoSDR(pixel)

    outputPixel(pixel)
```

**5. Hardware Considerations:**

*   **Encoding/Decoding Hardware:**  Requires powerful encoding/decoding hardware to handle variable bitrate/resolution streams in real-time.
*   **Display Hardware:** The local display should ideally support variable resolution rendering to avoid visual artifacts.
*   **Eye Tracking/Intent Prediction:**  Integrated with VR/AR headsets or uses specialized camera systems.