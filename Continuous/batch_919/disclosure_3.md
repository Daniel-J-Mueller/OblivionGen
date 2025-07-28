# 9500852

## Electrowetting Array with Integrated Microfluidic Logic

**Specification:**

**I. Core Concept:** Integrate microfluidic channels *within* the electrowetting device array, utilizing the same electrical control signals to manipulate both the electrowetting behavior *and* the flow of fluids through these integrated channels. This creates a hybrid electrowetting/microfluidic system capable of complex on-chip fluid handling and display functionalities.

**II. Materials:**

*   **Substrate:** Glass or flexible polymer (e.g., PDMS).
*   **Electrode Materials:** ITO (Indium Tin Oxide) or similar transparent conductive oxide.
*   **Dielectric Layer:** SiOCF or similar low-k dielectric (as per the original patent), but with embedded microchannel features.
*   **Microchannel Material:**  Photocurable polymer (e.g., Norland Optical Adhesive) or PDMS, directly patterned into the dielectric layer.
*   **Fluid Compatibility:**  All materials must be compatible with a range of aqueous and organic fluids.

**III.  Device Architecture:**

1.  **Electrowetting Array:** A standard electrowetting pixel array is fabricated on the substrate, as described in the original patent. Each pixel consists of a transparent electrode, a low-k dielectric layer, and a hydrophobic coating.
2.  **Integrated Microchannels:**  *Before* depositing the hydrophobic coating, microchannels are patterned *into* the dielectric layer using photolithography and etching or, preferably, directly fabricated during dielectric layer deposition via a lift-off process.  These channels run *between* adjacent electrowetting pixels, or even *through* the electrodes themselves (with appropriate insulation).
3.  **Control Scheme:** The same electrical signals used to control the electrowetting effect (i.e., to change the contact angle of the fluid on the surface) are also used to drive microfluidic pumps or valves within the integrated channels. This is achieved through:
    *   **Electrowetting-Driven Pumping:**  Adjacent pixels in the microchannel are independently controlled. By selectively activating/deactivating pixels, a traveling wave of surface tension is created, driving fluid flow through the channel.
    *   **Electrostatic Valve Control:** Small gaps are incorporated into the microchannels. By applying a voltage between adjacent electrodes, the gap can be electrostatically closed or opened, acting as a valve.
4.  **Pixel Connectivity:** Each electrowetting pixel is connected to a corresponding microchannel segment. This allows fluid to be selectively routed between pixels, enabling complex fluidic operations.

**IV. Functional Specifications & Pseudocode:**

1.  **Fluid Routing:**
    *   *Input:* Destination Pixel Coordinates (x,y).
    *   *Process:*
        1.  Calculate shortest path through the electrowetting array to destination pixel.
        2.  Activate/Deactivate appropriate electrowetting pixels to create a fluidic channel along the calculated path.
        3.  Apply voltage to adjacent pixels to drive fluid flow along the channel.
2.  **Mixing & Reaction Control:**
    *   *Input:* Reagent A & Reagent B, desired reaction time.
    *   *Process:*
        1.  Route Reagent A & Reagent B to a designated mixing chamber (a specific electrowetting pixel or a microchannel section).
        2.  Control mixing rate by adjusting the voltage applied to adjacent pixels (faster voltage changes = faster mixing).
        3.  Monitor reaction progress via optical detection (integrated sensors).
        4.  Terminate reaction by routing the product away from the mixing chamber.
3.  **Display Control:**
    *   *Input:* Image Data.
    *   *Process:*
        1.  Map each pixel in the image to a corresponding electrowetting pixel.
        2.  Apply appropriate voltage to each electrowetting pixel to control the contact angle of the fluid and create the desired image.
        3.  Use the microchannels to dynamically adjust fluid distribution for enhanced contrast or color mixing.

**V. Future Considerations:**

*   **Integrated Sensors:** Incorporate optical, chemical, or pressure sensors directly into the device for real-time monitoring of fluid properties.
*   **3D Microfluidics:** Extend the design to create multi-layered microfluidic networks for more complex operations.
*   **Closed-Loop Control:** Implement feedback control algorithms to precisely regulate fluid flow and reaction conditions.