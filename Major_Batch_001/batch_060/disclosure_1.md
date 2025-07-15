# 10048486

## Dynamic Reflective Pixel Subdivision

**Concept:** Enhance display contrast and viewing angles by dynamically altering the ratio of specular to diffuse reflection *within* individual pixels, not just between pixels. This moves beyond static specular/diffuse partitioning.

**Specs:**

*   **Pixel Structure:** Each pixel consists of an array of micro-mirrors or micro-lenses positioned *above* the existing specular and diffuse reflector layers. These micro-structures are individually addressable.
*   **Micro-Mirror/Lens Material:**  Thin-film silicon or a polymer with high refractive index contrast. Dimensions: 5-10 microns across.
*   **Addressing:** Each micro-mirror/lens is connected to an individual Thin-Film Transistor (TFT) within the pixel. This allows for precise control of the angle of reflection/refraction.
*   **Control System:** A pixel controller integrates signals for both electrowetting actuation *and* micro-mirror/lens alignment. This controller must be able to simultaneously manage fluid displacement and reflection pattern.
*   **Reflection Modes:**
    *   **Full Specular:** All micro-mirrors aligned to reflect light directly. High brightness, narrow viewing angle.
    *   **Full Diffuse:** All micro-mirrors angled to scatter light.  Wider viewing angle, lower brightness.
    *   **Graded Reflection:**  Micro-mirrors partially aligned to create a gradient between specular and diffuse reflection. This can be tuned to optimize contrast and viewing angle for specific content.
*   **Electrowetting Integration:** Electrowetting fluid controls the transparency of the pixel, modulating the overall brightness.  The micro-mirror/lens array controls the *direction* of the light.
*   **Layer Stack (from bottom to top):**
    1.  First Support Plate
    2.  Electrode Layer
    3.  Specular Reflector
    4.  Diffuse Reflector
    5.  Micro-mirror/Lens Array
    6.  Transparent Electrode Layer (for fluid control)
    7.  Second Support Plate

**Pseudocode (Pixel Controller):**

```
function updatePixel(brightnessLevel, viewingAngle) {
  // brightnessLevel: 0-100 (percentage)
  // viewingAngle: angle in degrees (0-180)

  // Calculate specular/diffuse ratio based on brightnessLevel and viewingAngle
  specularRatio = calculateSpecularRatio(brightnessLevel, viewingAngle);
  diffuseRatio = 1 - specularRatio;

  // Adjust each micro-mirror/lens based on the calculated ratios
  for each microMirror in microMirrorArray {
    mirrorAngle = calculateMirrorAngle(specularRatio, diffuseRatio, microMirror.position);
    setMirrorAngle(microMirror, mirrorAngle);
  }

  // Control electrowetting fluid to set overall brightness
  setFluidLevel(brightnessLevel);
}

function calculateSpecularRatio(brightness, angle) {
  // Implementation details depend on desired visual characteristics
  // This is a placeholder - tune parameters based on experimentation
  if (angle > 90) {
    return 0.2; //Prioritize diffusion for wider viewing
  }
  else {
    return brightness / 100;
  }
}
```

**Potential Benefits:** Increased contrast, wider viewing angles, improved image quality, dynamic optimization for different content types.