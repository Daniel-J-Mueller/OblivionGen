# 9482858

## Dynamic Pixel Wall Height Control - Microfluidic Actuation

**Concept:** Integrate microfluidic channels *within* the pixel walls themselves, allowing for dynamic adjustment of their height during operation. This facilitates not just fluid flow *around* the walls but also actively *modifies* the pixel geometry.

**Specs:**

*   **Pixel Wall Material:** Multi-layer material. Base layer is a rigid photoresist (as per claim 6). Overlaying this is a thin, flexible membrane (e.g., PDMS or similar elastomer). This membrane forms the visible pixel wall height.
*   **Microfluidic Channels:** Etched *within* the photoresist base layer *below* the flexible membrane. These channels run the length of each pixel wall (or sections thereof).
*   **Actuation Fluid:** A low-viscosity, electrically non-conductive fluid is pumped into/out of the microfluidic channels.
*   **Channel Network:** Each pixel wall has at least two distinct microfluidic channels: one for raising the wall height (filling with fluid) and one for lowering it (removing fluid).
*   **Control System:** Individual micro-pumps/valves (MEMS-based) are integrated for each pixel (or row/column of pixels for simplification). These control fluid flow to the pixel wall channels.
*   **Electrode Integration:** Transparent electrodes (ITO or similar) are patterned *on top* of the flexible membrane, enabling electrowetting control as in the original patent.
*   **Spacer Redundancy:** Spacers (claim 2, 3, 4, 8, 9, 14) *must* maintain a minimum gap even at maximum pixel wall deflation, preventing direct contact with the opposing substrate.
*   **Fluid Compatibility:** Actuation fluid must be chemically compatible with both the first (oil) and second (electrolyte) fluids used in the display.
*    **Wall Geometry:** The flexible membrane cross-section is not necessarily rectangular. Consider a triangular or curved profile for enhanced fluid movement and light scattering.

**Pseudocode (Control Algorithm):**

```
//For each pixel
{
    //Based on display image data for this pixel
    {
        If (pixel_intensity > threshold)
        {
            //Raise pixel wall height
            Activate_Pump(Pixel_ID, Raise_Channel, Flow_Rate_High);
            Activate_Pump(Pixel_ID, Lower_Channel, Flow_Rate_Zero);
        }
        Else
        {
            //Lower pixel wall height
            Activate_Pump(Pixel_ID, Raise_Channel, Flow_Rate_Zero);
            Activate_Pump(Pixel_ID, Lower_Channel, Flow_Rate_Medium);
        }

        //Dynamic Adjustment
        Read_Sensor(Pixel_ID, Wall_Height);
        If (Wall_Height < Minimum_Height) {
            Set_Pump(Pixel_ID, Raise_Channel, Corrective_Flow);
        }
        If (Wall_Height > Maximum_Height) {
            Set_Pump(Pixel_ID, Lower_Channel, Corrective_Flow);
        }

        //Electrowetting Control
        Apply_Voltage(Pixel_ID, Electrowetting_Electrode, Intensity_Level);
    }
}
```

**Potential Advantages:**

*   **Improved Contrast:** Dynamic height adjustment alters light scattering and viewing angle, potentially boosting contrast.
*   **Enhanced Fluid Control:** Precise control over pixel geometry allows for more sophisticated fluid manipulation.
*   **Novel Display Effects:** Creation of 3D-like effects or dynamic textures by varying pixel wall heights.
*   **Self-Cleaning:**  Briefly raising and lowering walls could dislodge contaminants.