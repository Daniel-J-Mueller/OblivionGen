# 11541322

**Adaptive Terrain for Magnetic Interaction Surfaces**

**Concept:** Expand the flat surface interaction paradigm to dynamic, reconfigurable 3D terrain. Instead of a purely planar surface for magnetic manipulation, create a surface comprised of individually controlled micro-actuators capable of raising and lowering sections, creating hills, valleys, and other topographical features. This allows for more complex interactions, puzzles, and game mechanics.

**Specs:**

*   **Surface Material:** Layered construction.
    *   Base Layer: Rigid PCB providing power and data lines to actuators.
    *   Actuator Layer: Array of MEMS-based electromagnetic actuators (piezoelectric or similar). Each actuator controls a small “tile” (e.g., 5mm x 5mm). Density: 100 actuators/cm^2. Each actuator has a range of motion of ±2mm.
    *   Surface Tiles: Small, magnetically receptive tiles (e.g., steel or ferrite) affixed to the top of each actuator. These tiles form the interactive surface. Tile Material: Durable plastic with magnetic coating.
*   **Control System:**
    *   Microcontroller: High-speed microcontroller to manage actuator array.
    *   Position Sensing: Capacitive or optical sensors to provide closed-loop control of actuator positions and ensure precise terrain shaping.
    *   Communication: Wireless communication (Bluetooth/Wi-Fi) for receiving commands from a remote device.
*   **Magnetic Objects:** Objects designed to be manipulated on the surface, containing permanent magnets or electromagnets. Object sizes: Variable (1cm to 10cm diameter).
*   **Software/API:**
    *   Terrain Editor: Software allowing users to design custom terrains.
    *   API: Library for controlling the actuator array and implementing game logic.
*   **Power:** External power supply (5V DC).

**Operation:**

1.  User designs a terrain in the Terrain Editor or loads a pre-defined terrain.
2.  Software translates terrain data into actuator control signals.
3.  Microcontroller drives actuators to create the desired terrain.
4.  User interacts with magnetic objects on the terrain via remote device.
5.  Actuators respond to object movements and user commands, creating dynamic interactions.

**Pseudocode (Terrain Shaping):**

```
function shapeTerrain(terrainData):
  for each tile in terrainData:
    tileHeight = terrainData[tile].height
    tileAddress = terrainData[tile].address
    targetPosition = tileHeight  //Target Z-axis position of the tile
    currentPosition = readSensor(tileAddress)  //Current Z-axis position of the tile
    error = targetPosition - currentPosition
    output = PIDController(error)  //Calculate control signal for actuator
    driveActuator(tileAddress, output)
end function
```

**Novelty & Potential:**

*   Moves beyond 2D flat surface interaction.
*   Creates immersive and dynamic puzzle/game experiences.
*   Allows for realistic simulations (e.g., miniature golf, tabletop war games).
*   Offers potential for haptic feedback via precise actuator control.
*   Adaptable to different surface sizes and resolutions.