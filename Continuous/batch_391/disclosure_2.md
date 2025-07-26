# 12022075

## Dynamic Perceptual Mapping for Multi-Display Environments

**Concept:** Extend the perceptual quantization mapping beyond single display characteristics to account for diverse viewing environments. Current systems calibrate for a specific display; this proposes a dynamic adjustment of the perceptual mapping based on detected ambient lighting and viewer distance, to optimize the perceived image quality across a range of conditions.

**Specs:**

*   **Sensor Integration:** Integrate ambient light sensors (RGB + Lux) and depth sensors (ToF or Stereo Vision) into viewer devices (phones, TVs, VR headsets).
*   **Perceptual Model Database:** Create a database of perceptual models parameterized by ambient light levels (0-1000 lux, in 10 lux steps) and viewing distances (0.2m – 5m, in 0.1m steps). These models define the optimal perceptual quantization curves.
*   **Real-time Environment Analysis:**  The viewer device continuously monitors ambient light and estimates viewing distance.
*   **Dynamic Mapping Selection:** Based on the detected environment, the system selects the closest matching perceptual model from the database. This model dictates the perceptual quantization curve used for encoding and decoding.
*   **Encoding Pipeline Modification:** Integrate the selected perceptual quantization curve into the encoding process. This involves adjusting the electro-optical transfer function (EOTF) applied to the luminance values *before* encoding.
*   **Decoding Pipeline Modification:** The decoding pipeline *also* uses the selected perceptual quantization curve.  This ensures consistency between encoding and decoding, and optimizes the image for the actual viewing conditions.
*   **Metadata Transmission:**  Transmit the selected perceptual model index (or a compressed representation) as metadata alongside the encoded video/image.  This allows the decoding device to use the correct quantization curve.
*   **Adaptive Learning:** Implement a machine learning component that learns and refines the perceptual models over time, based on user feedback (e.g., brightness/contrast adjustments).

**Pseudocode (Encoding):**

```
function EncodeFrame(frame, perceptualModelIndex):
  ambientLight = GetAmbientLight()
  viewingDistance = GetViewingDistance()
  perceptualModel = GetPerceptualModel(ambientLight, viewingDistance) // Retrieves from database

  for each pixel in frame:
    luminance = pixel.luminance
    brightness = ApplyNonPerceptualEOTF(luminance)
    perceptualValue = ApplyPerceptualEOTF(brightness, perceptualModel) // Uses the selected model
    encodedValue = Quantize(perceptualValue)
    pixel.encodedValue = encodedValue

  AddMetadata(encodedFrame, perceptualModelIndex)
  return encodedFrame
```

**Pseudocode (Decoding):**

```
function DecodeFrame(encodedFrame, metadata):
  perceptualModelIndex = GetPerceptualModelIndex(metadata)
  perceptualModel = GetPerceptualModel(perceptualModelIndex)

  for each pixel in encodedFrame:
    decodedValue = pixel.encodedValue
    perceptualValue = DeQuantize(decodedValue)
    brightness = ApplyInversePerceptualEOTF(perceptualValue, perceptualModel)
    luminance = ApplyInverseNonPerceptualEOTF(brightness)
    pixel.luminance = luminance

  return decodedFrame
```

**Further considerations:**

*   Develop a compact representation for the perceptual models to minimize metadata overhead.
*   Investigate the use of neural networks to dynamically generate perceptual quantization curves based on the environment.
*   Extend the system to account for display characteristics (e.g., color gamut, contrast ratio).
*   Explore the potential of using foveated rendering in conjunction with dynamic perceptual mapping to further optimize image quality and reduce bandwidth.