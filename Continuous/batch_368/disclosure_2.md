# 9728675

## Microfluidic Lens Array with Dynamic Aperture Control

**Concept:** Integrate microfluidic channels *within* the sidewall structure of the display substrate to create a dynamic lens array. These lenses, filled with a conductive fluid, will modulate light transmission at the pixel level, allowing for precise control of both brightness and focal plane *without* altering the core electro-wetting switching mechanism. This allows for a 3D or holographic display effect.

**Specs:**

*   **Sidewall Material:** Transparent polymer (e.g., PDMS, PMMA) with embedded microfluidic channels. Channels must be chemically compatible with the electrolyte and conductive fluids used in the electro-wetting display.
*   **Channel Dimensions:** 
    *   Width: 5-20μm (determines lens aperture)
    *   Height: 2-10μm (determines focal length)
    *   Spacing: 10-50μm (determines lens density)
*   **Fluid Composition:**
    *   Conductive Fluid:  An oil-based electrolyte solution with a high refractive index. The conductive properties must be compatible with the pixel electrode.
    *   Control Fluid: A secondary, immiscible fluid (e.g., water or a different oil) used to drive the conductive fluid within the channels.
*   **Actuation:** Individual micro-pumps or electro-osmotic actuators integrated beneath the substrate to control fluid flow within each channel.  Actuators are addressable via the same control lines as the pixel electrodes.
*   **Lens Configuration:** Each channel forms a meniscus lens when filled with the conductive fluid. By controlling the fluid volume in each channel, the curvature (and therefore the focal length) of each lens can be dynamically adjusted.
*   **Light Blocking Layer Adaptation:** The hydrophilic light blocking layer is modified to include micro-apertures corresponding to the lens locations, allowing light to pass through the lenses.

**Operation:**

1.  The display substrate is fabricated with the embedded microfluidic channels within the sidewalls.
2.  The hydrophobic and hydrophilic layers are deposited, incorporating micro-apertures for the lenses.
3.  The microfluidic channels are filled with the conductive fluid.
4.  By applying voltage to the micro-pumps/actuators, the volume of conductive fluid within each channel is adjusted.
5.  Changing the fluid volume alters the curvature of the meniscus lens, modulating the amount of light transmitted through that pixel.

**Pseudocode for Control System:**

```
// Define Pixel Array dimensions
pixel_width = 100;
pixel_height = 100;

// Define Lens Array parameters
lens_diameter = 10;
lens_spacing = 20;

// Initialize lens control array
lens_control[pixel_width][pixel_height] = 0; // 0 = lens off, 1-10 = lens intensity/focal length

// Function to update lens control
function update_lens(x, y, intensity) {
  lens_control[x][y] = intensity;
  send_signal_to_micro_pump(x, y, intensity); //Control actuator for pixel(x,y)
}

// Main Display Loop
while (true) {
  // Read pixel data from display buffer
  // For each pixel (x, y):
  //   brightness = pixel_buffer[x][y];

  //   //Calculate lens intensity based on pixel brightness
  //   lens_intensity = map(brightness, 0, 255, 0, 10);

  //   update_lens(x, y, lens_intensity);
}
```

This system creates an array of controllable lenses which operate in parallel with the electro-wetting display, allowing for complex optical effects and potentially creating a true 3D display without the need for glasses.