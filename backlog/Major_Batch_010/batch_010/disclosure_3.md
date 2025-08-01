# 10268035

**Microfluidic Lens Array with Electrowetting Control**

**Concept:** Integrate microfluidic lenses directly into the electrowetting display architecture. Instead of merely controlling fluid flow *between* pixels, utilize electrowetting to dynamically shape microfluidic lenses *within* each pixel, creating a focusable display. This moves beyond simple on/off switching to active optical manipulation.

**Specs:**

*   **Lens Material:** A clear, conductive fluid (e.g., a liquid metal alloy dispersed in a non-conductive carrier fluid) encapsulated within microfluidic channels formed on the second base substrate.
*   **Channel Dimensions:** Channels will be approximately 50-200 microns in diameter, with a parabolic or aspherical cross-section optimized for focusing light. Channel length will correspond to pixel size.
*   **Electrode Configuration:** Each microfluidic lens will be surrounded by individual electrodes patterned on the second base substrate. These electrodes will control the electrowetting force on the conductive fluid, deforming the lens shape. A common electrode will still exist.
*   **Electrowetting Layer:** The electrowetting layer will interface directly with the conductive fluid within the microfluidic channels. A dielectric coating will be necessary to prevent short circuits.
*   **Control Algorithm:** A real-time control algorithm will modulate the voltage applied to each electrode, adjusting the lens shape to achieve desired focusing and image correction. Pseudocode:

    ```
    For each pixel:
        Read target image data
        Calculate required lens shape based on image data and desired focus
        Apply voltage to individual electrodes to achieve calculated lens shape
        Monitor lens shape via optical feedback (e.g., interferometry)
        Adjust voltage as needed to maintain target shape
    ```
*   **Column Spacer Adaptation:** Existing column spacers will be modified to maintain precise channel height and prevent channel collapse under electrowetting force. The spacers will now be cylindrical and made of a robust polymer.
*   **Fluid Sealing:** Microfluidic channels must be perfectly sealed to prevent fluid leakage. This will be achieved using a combination of hydrophobic coatings and precision microfabrication techniques.
*   **Materials:** Second base substrate made of glass, column spacers made of PMMA, microfluidic channels made of PDMS.

**Innovation Details:**

This system moves beyond passive displays. Individual pixel focusing allows for:

*   **3D Displays:** By independently controlling the focal point of each pixel, a true 3D image can be generated without the need for special glasses.
*   **Dynamic Depth of Field:** The depth of field can be dynamically adjusted, allowing users to focus on specific areas of the image.
*   **Adaptive Optics:** The system can compensate for distortions caused by viewing angle or lens imperfections, resulting in a sharper, more accurate image.
*   **Variable Pixel Size/Resolution:** By controlling the shape of the lens, the effective pixel size can be changed, allowing for variable resolution displays.

**Further Development:**

*   Integration of optical sensors for real-time feedback and adaptive control.
*   Development of advanced control algorithms for complex image manipulation.
*   Exploration of different materials and fabrication techniques to optimize performance and cost.
*   Development of a full-color microfluidic lens array.