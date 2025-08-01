# 10133058

## Variable-Compliance Spacer Arrays for Dynamic Electrowetting Displays

**Concept:** Integrate microfluidic channels *within* the spacers themselves, allowing for localized adjustment of fluid pressure and thus, precise control over droplet shape and movement *beyond* simple on/off switching. This moves beyond static pixel boundaries.

**Specifications:**

*   **Spacer Material:** Borosilicate glass with etched microchannels (channel width: 50-200um, depth 20-50um). Alternatively, a photopolymer resin capable of microchannel fabrication.
*   **Microchannel Network:** Each spacer incorporates a closed-loop microfluidic network. Networks are interconnected across the spacer array via nano-valves (MEMS-based or electro-wetting controlled).
*   **Fluid Reservoir:** A thin-film reservoir integrated into the rear of the second support plate, containing a dielectric fluid compatible with the electrowetting fluids.
*   **Actuation:** Piezoelectric micro-pumps integrated into the second support plate near each spacer to drive fluid circulation within the microchannels. Control signals are delivered via ITO traces patterned on the second support plate.
*   **Spacer Geometry:** Spacers are not uniform. They are constructed with variable heights. Heights are dynamically adjustable through localized fluid pressure in the microchannels (10-100um range).
*   **Electrowetting Fluid:** Standard electrowetting fluids.
*   **Control System:** FPGA-based controller with high-speed PWM outputs for piezoelectric pump control and electrowetting signal generation.
*   **Software Interface:** Python-based GUI for defining droplet shapes, movement paths, and dynamic spacer profiles.

**Operational Pseudocode:**

```python
# Define display resolution (x, y)
x_res = 640
y_res = 480

# Initialize spacer array (2D array representing spacer heights)
spacer_array = [[0 for _ in range(x_res)] for _ in range(y_res)]

# Function to set spacer height at (x, y)
def set_spacer_height(x, y, height):
    spacer_array[y][x] = height
    # Send command to microfluidic pump to adjust pressure in corresponding spacer channel

# Function to define droplet shape (e.g., circle, square, custom)
def define_droplet_shape(x, y, shape, parameters):
    # Calculate electrowetting voltage required to achieve desired shape based on spacer height and fluid properties
    voltage = calculate_voltage(spacer_height=spacer_array[y][x], shape=shape, parameters=parameters)
    # Apply voltage to corresponding pixel electrode

# Main loop
while True:
    # Read user input (droplet position, shape, height)
    droplet_x, droplet_y, droplet_shape, droplet_parameters = get_user_input()

    # Adjust spacer height at droplet position
    set_spacer_height(droplet_x, droplet_y, desired_height)

    # Apply electrowetting voltage to achieve desired droplet shape
    define_droplet_shape(droplet_x, droplet_y, droplet_shape, droplet_parameters)
```

**Innovation:** This system moves beyond passive pixel boundaries. Active control of spacer height *coupled* with electrowetting enables the creation of highly complex droplet shapes and dynamic displays capable of 3D effects, variable contrast, and improved viewing angles. The closed-loop microfluidic system ensures precise and repeatable control over spacer height, while the integrated piezoelectric pumps provide fast response times. This allows for fluidic sculpting.