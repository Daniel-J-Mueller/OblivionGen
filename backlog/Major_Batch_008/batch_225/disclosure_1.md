# 9585277

## Micro-Lens Array for Enhanced Color Mixing in Electrowetting Displays

**Concept:** Integrate a micro-lens array (MLA) directly onto the first support plate, positioned *above* each picture element, to precisely direct ambient light through the switching fluid layers. This allows for improved color mixing and perceived brightness, particularly in multi-color electrowetting displays. The MLA design will be dynamic, utilizing individual, controllable micro-mirrors to further refine light direction.

**Specs:**

*   **Micro-Lens Material:** Transparent polymer with high refractive index (e.g., cyclic olefin copolymer - COC) for minimal light loss.
*   **Lens Dimensions:** Individual lenses 50-100 µm diameter, with focal length optimized for the thickness of the fluid layers and the target viewing angle.
*   **Lens Array Geometry:** Hexagonal close-packed arrangement for maximum fill factor and minimal gaps. Each lens corresponds to a single picture element (or sub-pixel in a color display).
*   **Micro-Mirror Integration:** Each lens is paired with a micro-mirror, fabricated from reflective material (e.g., aluminum) and actuated by micro-electromechanical systems (MEMS). Mirror tilt controlled via individual electrodes.
*   **Electrode Material:** Indium Tin Oxide (ITO) patterned on the first support plate, beneath the micro-mirrors.
*   **Control System:** A dedicated control circuit for addressing and controlling the tilt of each micro-mirror.  Algorithm for dynamic adjustment based on ambient light conditions and desired display characteristics.
*   **Layer Stack:**
    1.  First Support Plate (containing electrodes for both picture elements *and* micro-mirrors)
    2.  Micro-Mirror Array
    3.  Micro-Lens Array
    4.  Fluid Layers (first and second fluid)
    5.  Second Support Plate
*   **Addressing Scheme:** Row/column addressing using thin-film transistors (TFTs) for individual mirror control.
*   **Manufacturing Process:**
    1.  Fabricate first support plate with integrated electrodes.
    2.  Deposit and pattern reflective material for micro-mirrors.
    3.  Mold or etch micro-lenses onto the first support plate, aligning with micro-mirrors and picture elements.
    4.  Complete display assembly with fluid layers and second support plate.

**Pseudocode (Dynamic Brightness Adjustment):**

```
function adjustBrightness(ambientLightLevel, desiredBrightness) {
  for each microMirror in mirrorArray {
    mirrorAngle = calculateMirrorAngle(ambientLightLevel, desiredBrightness, microMirror.position);
    setMirrorAngle(microMirror, mirrorAngle);
  }
}

function calculateMirrorAngle(ambientLightLevel, desiredBrightness, mirrorPosition) {
  // Algorithm to determine optimal mirror angle based on:
  // - Ambient light level (sensor input)
  // - Desired brightness (user setting)
  // - Mirror position within the array
  // - Fluid properties (refractive index, viscosity)
  // - Viewing angle

  // Incorporate a dynamic lookup table or machine learning model to optimize angles
  // based on real-time conditions and user preferences.

  // Example:
  if (ambientLightLevel > threshold) {
    mirrorAngle = reduceAngleToDimDisplay(mirrorAngle);
  } else {
    mirrorAngle = increaseAngleToBrightenDisplay(mirrorAngle);
  }

  return mirrorAngle;
}
```

**Innovation:** The MLA, coupled with individually controlled micro-mirrors, enables precise light management within the electrowetting display.  This significantly enhances color mixing, contrast, and perceived brightness, while also offering dynamic control over viewing angles and ambient light adaptation.  This addresses a key limitation of current electrowetting displays, which often suffer from lower brightness and color saturation compared to other display technologies.