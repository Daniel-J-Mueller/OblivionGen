# 11796819

## Dynamic Focal Plane Microscopy with Metasurface Arrays

**Concept:** Integrate the liquid crystal metasurface technology with a dynamic focal plane microscopy approach to create a high-speed, high-resolution 3D imaging system. Instead of mechanically scanning a focal plane, actively reshape the metasurface to steer the focal point through the sample volume.

**Specifications:**

*   **Metasurface Array:** Fabricate a two-dimensional array of individually addressable liquid crystal metasurface elements (pixels). Each element will act as a miniature lens capable of independent phase and amplitude modulation. Resolution: 128x128 elements minimum, ideally 512x512.
*   **Electrode System:** Utilize a high-density electrode array positioned behind each metasurface element. Employ thin-film transistors (TFTs) for precise voltage control and rapid switching. Switching time target: <10 microseconds per element.
*   **Illumination:** Utilize a high-brightness LED or laser source emitting light within the visible spectrum (400-700nm). Consider structured illumination to reduce scattering and improve image contrast.
*   **Waveguide Integration:** Incorporate a multi-mode optical waveguide directly beneath the metasurface array. The waveguide serves to collect light from the focused spot and deliver it to a high-speed image sensor.
*   **Image Sensor:** Employ a CMOS image sensor with high frame rate (>1000 fps) and high quantum efficiency. Sensor resolution: 512x512 pixels minimum.
*   **Control System:** Develop a real-time control algorithm to coordinate the voltage applied to each electrode. Algorithm must account for diffraction effects, aberrations, and the optical properties of the sample.
*   **Sample Stage:** Integrate the system with a precision XYZ stage for coarse sample positioning.

**Operational Pseudocode:**

```
// Initialization
Initialize Metasurface Array
Initialize Electrode Control System
Initialize Image Sensor
Initialize Sample Stage

// 3D Scan Loop
while (scan_complete == false) {
    // Define Scan Parameters (step size, scan volume)
    scan_volume = ...
    step_size = ...

    // Iterate through Scan Volume
    for (x in scan_volume.x) {
        for (y in scan_volume.y) {
            for (z in scan_volume.z) {

                // Calculate Electrode Voltages for Focal Point at (x, y, z)
                voltages = calculateVoltages(x, y, z)

                // Apply Voltages to Electrode Array
                applyVoltages(voltages)

                // Capture Image from Image Sensor
                image = captureImage()

                // Process Image (noise reduction, contrast enhancement)
                processedImage = processImage(image)

                // Store Processed Image with corresponding (x, y, z) coordinates
                storeImage(processedImage, x, y, z)
            }
        }
    }

    // Reconstruct 3D Volume from Stored Images
    reconstructVolume()

    scan_complete = true
}
```

**Potential Applications:**

*   High-speed 3D imaging of biological samples
*   Real-time volumetric microscopy for surgical guidance
*   Non-destructive 3D inspection of materials
*   Advanced optical metrology