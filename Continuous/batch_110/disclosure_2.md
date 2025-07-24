# 10785495

## Temporal Distortion via Chromatic Aberration Encoding

**Concept:** Instead of embedding timecode *as* data, embed it as a controlled, visually perceptible distortion of the image itself – specifically, chromatic aberration. Modulate the red/green/blue channel offsets *based* on the timecode, creating a subtle, evolving “ripple” across the video frame. This is a form of steganography, but rather than hiding a *message*, it’s hiding time.

**Specs:**

*   **Encoding Resolution:**  Timecode granularity to 1/100th of a second.
*   **Aberration Vector:** Define a 2D vector field representing the chromatic aberration offset for each pixel. This vector field will be dynamically generated based on the current timecode value.
*   **Modulation Function:**  A mathematical function that maps timecode values to aberration vector magnitudes and directions. This function must be tunable to control the visual intensity of the distortion.  Consider a sinusoidal function for cyclical effects, or a stepped function for discrete timecode values.
*   **Spatial Distribution:**  The aberration effect should not be uniform. Introduce a spatial gradient or pattern. One option is a radial gradient emanating from the center of the frame, or a linear gradient along one edge. This helps to distinguish the timecode encoding from other image artifacts.
*   **Channel Mapping:**  Map timecode components (hours, minutes, seconds, frames) to individual color channels.  Example: Hours -> Red offset, Minutes -> Green offset, Seconds -> Blue offset.  Sub-second values can modulate the *magnitude* of the offset for all channels.
*   **Dynamic Range:** Limit the maximum aberration offset to a visually perceptible but non-destructive level. The goal is to create a subtle, almost subliminal effect.  Calibration will be critical.
*   **Decoding Process:** An algorithm that samples the chromatic aberration in defined regions of the frame and reconstructs the timecode value. This requires knowledge of the encoding function and calibration parameters.
*   **Frame Rate Dependency:** Ensure the encoding and decoding algorithms are frame rate agnostic.

**Pseudocode (Encoding):**

```
function encode_timecode(frame, timecode):
  // timecode is in format HH:MM:SS:FF (Hours, Minutes, Seconds, Frames)

  // Extract components
  hours = timecode.hours
  minutes = timecode.minutes
  seconds = timecode.seconds
  frames = timecode.frames

  // Calculate aberration offsets based on components
  red_offset = hours * scale_factor_red
  green_offset = minutes * scale_factor_green
  blue_offset = seconds * scale_factor_blue
  subframe_offset = frames * scale_factor_subframe

  // Create aberration vector field (example: radial gradient)
  for each pixel (x, y) in frame:
    distance = sqrt((x - center_x)^2 + (y - center_y)^2)
    aberration_magnitude =  (distance / max_distance) * subframe_offset
    aberration_angle = atan2(y - center_y, x - center_x)

    red_shift = cos(aberration_angle) * aberration_magnitude + red_offset
    green_shift = sin(aberration_angle) * aberration_magnitude + green_offset
    blue_shift = 0 + blue_offset // or some other mapping

    // Apply shift to pixel's color channels
    pixel.red = clamp(pixel.red + red_shift, 0, 255)
    pixel.green = clamp(pixel.green + green_shift, 0, 255)
    pixel.blue = clamp(pixel.blue + blue_shift, 0, 255)

  return frame
```

**Potential Applications:**

*   **Watermarking/Provenance:**  Robustly embed time-sensitive information into video footage for authentication and tracking.
*   **Stealth Timecode:** Conceal timecode for sensitive applications (e.g., security footage).
*   **Visual Metronome:** Create a subtle visual cue synchronized with the video's timeline.
*   **Artistic Effects:** Explore the aesthetic possibilities of controlled chromatic aberration.