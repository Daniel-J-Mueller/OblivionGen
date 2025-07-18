# 9223127

## Dynamic Fluidic Lens Array for Electrowetting Displays

**Concept:** Integrate microfluidic lensing capabilities *within* the electrowetting display pixel structure. Instead of simply controlling fluid movement for display contrast, leverage the fluid itself – and its refractive index – to actively focus/defocus light, creating a dynamic lens array layered on top of the existing display. This allows for not only display *of* information but also manipulation *of* light, opening possibilities for integrated AR/VR functionality or advanced beam steering.

**Specs:**

*   **Pixel Structure Modification:** Each electrowetting pixel will be subdivided into a central “display zone” and a surrounding “lens zone.” Both zones share the same two support plates and immiscible fluids.
*   **Lens Fluid Composition:** The fluid in the lens zone will be a dielectric fluid containing microparticles with controllable refractive index (e.g., through applied voltage, temperature change, or light exposure). Alternatively, utilize an electro-responsive fluid itself whose refractive index changes with applied field.
*   **Microparticle Control:** Microparticles within the lens zone will be suspended and manipulated using a network of individually addressable microelectrodes embedded *within* the first support plate, positioned beneath the lens zone. Applying different voltages to these electrodes will create electric field gradients, causing the microparticles to aggregate or disperse, locally altering the refractive index.
*   **Electrode Configuration:**  The microelectrodes in the lens zone will be arranged in a hexagonal or square grid pattern. The density of the grid is 200-500 microelectrodes per mm². Each microelectrode is insulated from the other and from the conductive layers.
*   **Addressing Scheme:**  Each microelectrode will have an individual address, allowing precise control over the local refractive index. Utilize a multiplexing scheme to reduce the number of control lines.
*   **Control Algorithm:** Develop a control algorithm that maps desired focal lengths/beam steering angles to specific voltage patterns applied to the microelectrodes. This algorithm will need to account for the fluid dynamics, the microparticle properties, and the pixel geometry.
*   **Layer Stack:**
    1.  Rear Support Plate (Glass or flexible substrate)
    2.  Microelectrode Array (ITO, or other transparent conductor) - etched to form individual electrodes
    3.  Alignment Layer (for fluidic control)
    4.  Second Fluid (Dielectric Fluid + Microparticles or electro-responsive fluid)
    5.  First Fluid (Display Fluid)
    6.  Front Support Plate (Transparent)
*   **Pixel Size:**  Standard electrowetting pixel size (e.g., 50-100 μm) with an additional 10-20 μm allocated to the lens zone surrounding the display zone.
*    **Drive Voltage:**  The drive voltage for the lens zone microelectrodes should be compatible with existing electrowetting display drive voltages (typically < 5V).
*   **Materials:** Transparent conductive oxides (TCOs), dielectric materials, immiscible fluids with appropriate refractive indices and dielectric properties, microparticles with controllable refractive index.



**Pseudocode (Control Algorithm):**

```
function calculate_electrode_voltages(desired_focal_length, pixel_coordinates):
  // Input: desired focal length (mm), pixel coordinates (x,y)
  // Output: Array of voltages for each microelectrode in the lens zone

  // 1. Calculate the required refractive index gradient based on the desired focal length
  refractive_index_gradient = calculate_gradient(desired_focal_length)

  // 2. Calculate the voltage required for each microelectrode to achieve the corresponding refractive index
  for each microelectrode in lens_zone:
    // Calculate the desired refractive index at the microelectrode location
    desired_refractive_index = refractive_index_gradient[microelectrode.x, microelectrode.y]

    // Use a calibration curve or model to determine the voltage required to achieve the desired refractive index
    voltage = calculate_voltage(desired_refractive_index)

    // Assign the voltage to the microelectrode
    microelectrode.voltage = voltage

  // Return the array of microelectrode voltages
  return microelectrode_voltages
```

**Potential Applications:**

*   **Integrated AR/VR Headsets:** Dynamic focusing and beam steering eliminates the need for mechanical focus adjustment, reducing size and complexity.
*   **Advanced Microscopy:** Real-time, software-controlled focusing for high-resolution imaging.
*   **Adaptive Optics:** Correction for image distortion caused by atmospheric turbulence or optical aberrations.
*   **Holographic Displays:** Precise control over light wavefronts for creating 3D images.