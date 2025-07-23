# 11892721

## Dynamic Light Field Projection via Layered LC Control

**Concept:** Extend the liquid crystal control demonstrated in the patent to create a volumetric display capable of projecting dynamic light fields. Instead of simply switching privacy/opacity, modulate the light *direction* emanating from the display itself, creating the illusion of 3D objects suspended in space.

**Specs:**

*   **Display Structure:** Multi-layered liquid crystal panel stack. Minimum of 3 layers, optimally 5-7. Each layer operates as a discrete ‘plane’ in the volumetric display.
*   **Electrode Configuration:** Each LC layer possesses a micro-lens array patterned onto the electrode surface. These micro-lenses are individually addressable via the thin-film transistor layer, allowing precise control of light deflection. Lens focal lengths are optimized for layer separation distance.
*   **LC Material:** Fast-switching cholesteric liquid crystal.  Chosen for its wide viewing angle and ability to efficiently scatter light. Layer doping with dichroic dyes to provide colour control.
*   **Backlight System:** High-resolution micro-LED array positioned behind the LC stack. Each micro-LED corresponds to a pixel in the display. This provides precise illumination control for each volumetric voxel.
*   **Polarization Control:**  Each LC layer incorporates a dynamically adjustable polarization filter. Filters can rotate to control the polarization of light passing through, enhancing the contrast and clarity of the volumetric image.
*   **Control System:** Dedicated FPGA-based processor with high-speed data transfer interfaces.
*   **Software API:** Enables developers to define volumetric scenes using a 3D scene graph. API includes functions for specifying light intensity, colour, and direction for each volumetric voxel.

**Pseudocode – Volumetric Scene Rendering:**

```
// Define Volumetric Scene
Scene = new Scene();

// Define Volumetric Voxel
Voxel = new Voxel(x, y, z, colour, intensity);
Scene.add(Voxel);

// For each layer in the display:
For (Layer = 0; Layer < NumLayers; Layer++)
{
    // Project voxels onto the current layer
    ProjectedVoxels = Project(Scene.Voxels, Layer);

    // For each projected voxel:
    For (Voxel in ProjectedVoxels)
    {
        // Calculate micro-lens angle to deflect light
        Angle = CalculateAngle(Voxel.x, Voxel.y, Voxel.z, Layer);

        // Apply voltage to corresponding micro-lens
        SetMicroLensAngle(Voxel.x, Voxel.y, Layer, Angle);

        // Set micro-LED intensity
        SetMicroLEDIntensity(Voxel.x, Voxel.y, Voxel.z, Voxel.intensity);

    }
}

// Update display
Display.update();
```

**Further Considerations:**

*   **Eye Tracking Integration:** Integrate eye tracking to dynamically adjust the displayed light field, maximizing the 3D effect.
*   **Haptic Feedback:** Combine the visual display with localized haptic feedback to allow users to "feel" the volumetric objects.
*   **Computational Optics:** Utilize computational optics techniques to correct for aberrations and distortions, improving the image quality.
*   **Power Management:** Optimize power consumption by dynamically adjusting the backlight intensity and LC switching frequency.