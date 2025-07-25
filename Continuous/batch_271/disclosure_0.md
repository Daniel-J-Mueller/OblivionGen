# 9349139

## Biodegradable Art Sample Substrate with Embedded Sensory Data

**Concept:** Expand on the 'degrading art sample' idea, not by *how* it degrades (ink, material), but *what* it becomes *during* degradation. Instead of a simple fading/disintegration, engineer the substrate itself to release sensory data as it breaks down, directly influencing the augmented reality experience.

**Specifications:**

*   **Substrate Material:**  A bio-polymer matrix (e.g., PLA infused with fungal mycelium and conductive polymers).  Mycelium growth rate and conductive polymer concentration are precisely controlled during manufacturing.
*   **Data Encoding:**  The mycelium network *is* the data storage.  Specific growth patterns (influenced by the initial substrate layout and polymer concentration) represent encoded data about the original artwork – color profiles, texture maps, lighting information.  The conductive polymers act as pathways for signal transmission.
*   **Degradation Trigger:** Moisture and ambient air initiate mycelium growth and polymer breakdown.  Degradation rate is tunable via substrate composition and a microscopic sealant layer.
*   **AR Integration:**
    *   User’s device (smartphone/tablet) equipped with a moisture sensor.
    *   As the substrate degrades, the moisture sensor detects increased humidity around the sample.
    *   This triggers the AR app to read the ‘data’ released during degradation. The app reads conductivity changes within the growing mycelium network, decoding data about the artwork.
    *   Decoding algorithms translate conductivity readings into texture, color, and lighting data for the AR overlay.
    *   AR overlay becomes *more* accurate and detailed as the sample degrades, reflecting the ‘unfolding’ of data.
*   **Sensor Array:** Integrate a micro-array of capacitive touch sensors into the substrate. As the mycelium grows, it interacts with the touch sensors, providing additional data about the texture and shape of the original artwork, enhancing the AR experience.
*   **Power Source:**  Bio-voltaic cells embedded within the substrate generate a small amount of power from the degradation process. This powers the capacitive touch sensors and may provide data transmission capabilities.

**Pseudocode (AR App Logic):**

```
FUNCTION processDegradationData(moistureLevel, touchSensorData, conductivityData):
  // moistureLevel: Value from device moisture sensor (0-100)
  // touchSensorData: Array of values from capacitive touch sensors
  // conductivityData: Array of values representing conductivity changes

  IF moistureLevel > threshold:
    // Decode data from conductivityData
    artworkColorProfile = decodeColor(conductivityData)
    artworkTextureMap = decodeTexture(conductivityData)

    //Process touch sensor data to refine texture
    refinedTextureMap = refineTexture(artworkTextureMap, touchSensorData)

    //Overlay AR image with decoded data
    displayARImage(refinedTextureMap, artworkColorProfile)
  ENDIF
END FUNCTION
```

**Innovation:** This moves beyond simply *visualizing* art in a space. It creates a dynamic AR experience that is intrinsically linked to the physical degradation of the sample, unlocking additional data about the artwork as it breaks down. The degradation process *becomes* the data transmission method.