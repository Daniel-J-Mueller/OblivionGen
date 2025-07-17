# D1035610

## Bio-Reactive Speaker Housing

**Concept:** A wireless speaker housing constructed from a bio-reactive material that subtly shifts color and texture in response to ambient sound and user interaction. The aim is to create a speaker that appears ‘alive’ and intimately connected to the audio it produces.

**Materials:**

*   **Base Matrix:** Mycelium composite – a rapidly renewable and biodegradable material offering structural integrity. The mycelium will be grown *around* a lattice of conductive polymers.
*   **Bio-luminescent Bacteria:** Genetically engineered *Vibrio fischeri* encapsulated within microfluidic channels embedded within the mycelium matrix.  These bacteria produce light in response to vibrations.
*   **Electro-active Polymers (EAPs):**  Integrated into the mycelium lattice, allowing for subtle shape changes and texture variations when stimulated electrically.
*   **Conductive Polymer Network:**  A fine network woven throughout the mycelium, providing both structural support and a means of delivering electrical signals to the EAPs and monitoring bacterial bioluminescence.
*   **Protective Coating:** A thin layer of biocompatible, transparent polymer to encapsulate the mycelium and protect the bio-components from environmental factors.

**Functionality:**

1.  **Sound-Reactive Bioluminescence:**  The speaker's internal audio signal will be fed into a microcontroller. This microcontroller will analyze the audio frequencies and amplitudes.  Low frequencies will trigger higher-intensity bioluminescence, while higher frequencies will trigger faster flickering patterns. The light will emanate from the mycelium housing.

    ```pseudocode
    function analyzeAudio(audioSignal):
      frequencySpectrum = FFT(audioSignal)
      lowFrequencyThreshold = 200 Hz
      highFrequencyThreshold = 2000 Hz

      for each frequency in frequencySpectrum:
        if frequency < lowFrequencyThreshold:
          bioluminescenceIntensity = map(frequency, 0, lowFrequencyThreshold, 10, 100) // Map to intensity range
          triggerBioluminescence(bioluminescenceIntensity)
        else if frequency > highFrequencyThreshold:
          flickerRate = map(frequency, highFrequencyThreshold, 20000, 1, 100) // Map to flicker rate
          triggerBioluminescenceFlicker(flickerRate)
        else:
          triggerBioluminescence(50) // Default intensity
    ```

2.  **Touch-Reactive Texture Change:** Capacitive sensors embedded in the speaker's surface will detect touch. Upon sensing touch, the EAPs will contract or expand, causing subtle ripples and changes in the speaker’s surface texture. Different touch areas can trigger different textural responses.

    ```pseudocode
    function handleTouch(touchLocation, pressure):
      if touchLocation == "top":
        activateEAP(topEAP, pressure * 0.5) //Adjust EAP activation based on pressure
      else if touchLocation == "side":
        activateEAP(sideEAP, pressure * 0.2)
      else:
        activateEAP(baseEAP, pressure * 0.1)
    ```

3.  **Ambient Light Adaptation:** A light sensor will monitor the ambient light level. The bioluminescence intensity will be automatically adjusted to complement the surrounding lighting, creating a more immersive experience.

4.  **Nutrient Delivery System:**  Microfluidic channels within the mycelium will allow for the periodic delivery of nutrients to sustain the bio-components.  This could be automated with a timed release system or triggered by user interaction (e.g., a button press).

**Aesthetic Considerations:**

*   The mycelium composite can be grown into a variety of organic shapes and textures.
*   The bioluminescence will create a soft, diffused glow, reminiscent of deep-sea creatures.
*   The textural changes will add a tactile dimension to the listening experience.
*   Colors are dictated by genetic engineering of the bacteria, but could include blues, greens, or even subtle oranges.

**Engineering Challenges:**

*   Maintaining the viability and stability of the bio-components over time.
*   Developing a reliable nutrient delivery system.
*   Ensuring the biocompatibility of all materials.
*   Balancing the aesthetic goals with the practical requirements of a wireless speaker.