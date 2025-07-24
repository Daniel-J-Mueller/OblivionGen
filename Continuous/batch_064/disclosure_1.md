# 10274720

## Micro-Lens Array for Enhanced Light Control & Individual Pixel Addressing

**Concept:** Integrate a micro-lens array (MLA) directly *within* the pixel structure, alongside the TFT, to achieve precise light direction and, crucially, *individual* pixel light modulation beyond simple on/off control. This builds on the light-blocking layer idea – instead of blocking light, we *redirect* it.

**Specs:**

*   **MLA Material:** Transparent polymer (e.g., PDMS) or glass, chosen for refractive index and moldability.  Must withstand manufacturing processes.
*   **Lenslet Dimensions:**  5-20um diameter, 2-10um focal length.  Precisely engineered for the pixel pitch and display viewing angle.
*   **Lenslet Array Configuration:**  Each pixel incorporates *multiple* (3-5) lenslets, arranged in a non-uniform pattern within the pixel area. The pattern is unique per pixel.
*   **TFT Integration:**  The TFT structure is partially embedded *under* the MLA, minimizing shadow cast and maximizing light transmission.  Thin-film encapsulation is crucial.
*   **Active Control:** Each lenslet’s focal length is dynamically adjustable via micro-electro-mechanical systems (MEMS) or liquid crystal manipulation. Each lenslet has an associated micro-actuator.
*   **Addressing:** Each lenslet is individually addressable via a dedicated control line integrated with the existing gate/source lines.  A modified driver circuit is required.
*   **Light-Blocking Layer Adaptation:** The existing light-blocking layer is redesigned as a 'light-shaping' layer. It incorporates micro-apertures and diffractive elements to further refine light direction and prevent cross-talk between pixels.
*   **Substrate:** Utilizes existing support plates, with modifications for MEMS integration and increased electrical routing density.

**Innovation Description:**

The core idea is to move beyond simple pixel illumination/blocking. By individually controlling the direction of light *within* each pixel, we can achieve several benefits:

1.  **Viewing Angle Enhancement:**  Steering light toward the viewer’s eye enhances perceived brightness and contrast, particularly at wider viewing angles.
2.  **Sub-Pixel Rendering:**  The micro-lens array effectively creates a dynamically adjustable sub-pixel grid *within* each physical pixel. This drastically increases effective resolution and color gamut.
3.  **3D Display Capabilities:**  By carefully controlling light direction from adjacent pixels, we can create a basic autostereoscopic 3D effect *without* the need for specialized glasses.
4.  **Privacy Features:** Directing light away from certain angles creates a privacy filter.
5.  **Adaptive Brightness:** Redirecting light internally to improve uniformity and efficiency.

**Pseudocode for Pixel Control:**

```
// Pixel Class
class Pixel {
  int pixelID;
  Lenslet[] lenslets;

  // Constructor
  Pixel(int id, Lenslet[] lensArray) {
    pixelID = id;
    lenslets = lensArray;
  }

  // Function to set lenslet focus
  void setLensletFocus(int lensletIndex, float focalLength) {
    lenslets[lensletIndex].setFocalLength(focalLength);
  }

  // Function to control pixel output
  void setPixelOutput(float brightness, float colorR, float colorG, float colorB) {
      // Control TFT to set basic pixel brightness and color.
      // Then, dynamically adjust lenslet focal lengths for sub-pixel rendering and view angle optimization
      for (int i = 0; i < lenslets.length; i++) {
          float focalAdjustment = calculateFocalAdjustment(brightness, colorR, colorG, colorB, i);
          setLensletFocus(i, focalAdjustment);
      }
  }
}

// Lenslet Class
class Lenslet {
    float focalLength;
    MEMSActuator actuator;

    Lenslet(MEMSActuator act) {
        actuator = act;
        focalLength = initialFocalLength;
    }

    void setFocalLength(float newFocalLength) {
        actuator.move(newFocalLength);
        focalLength = newFocalLength;
    }
}

// Calculate focal adjustment algorithm
float calculateFocalAdjustment(float brightness, float colorR, float colorG, float colorB, int lensletIndex) {
    // Algorithm based on desired pixel output and lenslet index
    // Utilize pre-computed look-up tables or machine learning models
    // Output: focal length adjustment value
}
```

**Manufacturing Challenges:**

*   Precise MLA fabrication and integration.
*   MEMS actuator reliability and long-term performance.
*   Increased complexity of driver circuits.
*   Ensuring uniform light transmission through the MLA.

**Potential Applications:**

*   High-resolution displays for VR/AR headsets.
*   Automotive displays with wide viewing angles.
*   Energy-efficient displays for mobile devices.
*   Flexible and foldable displays.