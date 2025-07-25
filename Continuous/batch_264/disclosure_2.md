# 9651771

**Microfluidic Emulsion Painting System for Dynamic Electrowetting Displays**

**System Overview:**

A robotic deposition system utilizing microfluidic printheads to create electrowetting displays with dynamically adjustable pixel characteristics. Instead of a fixed emulsion application as described in the reference patent, this system will precisely “paint” emulsions onto the support plate, allowing for localized control of fluid layer thicknesses and composition *after* initial device fabrication.  This allows for reconfiguration of display properties – contrast, color, viewing angle – in real-time.

**Component Specifications:**

1.  **Microfluidic Printhead Array:**
    *   Nozzle Diameter: 10-50 micrometers
    *   Nozzle Material: Chemically resistant polymer (e.g., PTFE)
    *   Nozzle Quantity:  Variable density array (100-1000 nozzles/cm²) – higher density for fine feature resolution.
    *   Fluid Control: Individual piezoelectric actuators for each nozzle, enabling precise droplet volume control (picoliter scale) and ejection frequency (up to 1 kHz).
    *   Heated Nozzle Assembly: Maintain emulsion viscosity and prevent clogging. Temperature control: 20-80°C.

2.  **Emulsion Delivery System:**
    *   Reservoirs: Separate reservoirs for first and second fluids.
    *   Micro-pump System:  Precise micro-pumps (syringe or peristaltic) to deliver fluids to the mixing chamber. Flow rate accuracy: +/- 1%.
    *   Micro-Mixer:  Passive or active micro-mixer to create a stable emulsion with controlled droplet size (see section 4).
    *   Emulsion Filter:  Inline filter to remove any large particles or air bubbles. Pore size: 1-5 micrometers.

3.  **Support Plate Handling System:**
    *   XY Stage: High-precision XY stage with travel range of at least 20cm x 20cm. Positioning accuracy: +/- 1 micrometer.
    *   Z-Axis Control: Precise Z-axis control for maintaining consistent distance between the printhead and the support plate.
    *   Vacuum Chuck: Securely hold the support plate during deposition.
    *   Heated Stage: Optional – maintain support plate temperature during deposition.

4.  **Emulsion Formulation Control:**
    *   Droplet Size Control: Utilize flow focusing techniques within the micro-mixer to generate emulsions with narrow droplet size distributions. Target range: 0.5 – 5 micrometers. Coefficient of Variation (CV) < 15%.
    *   Surfactant Control: Implement a library of non-ionic surfactants with varying HLB values to optimize emulsion stability and droplet size.
    *   Fluid Viscosity Control: Implement micro-heaters to adjust emulsion viscosity as needed.
    *   Real-time Monitoring: Incorporate micro-optical sensors (e.g., interferometry) to monitor droplet size and distribution during emulsion creation.

5.  **Control Software & Algorithm:**
    *   Image Processing: Import desired display patterns as images (e.g., BMP, PNG).
    *   Path Planning: Algorithm to generate optimized nozzle trajectories for depositing the emulsion according to the image pattern.
    *   Droplet Placement Control: Precise control of droplet placement and overlap to achieve desired pixel characteristics.
    *   Real-time Feedback: Integrate feedback from the optical sensors to adjust droplet size and placement in real-time.
    *   User Interface: Intuitive graphical user interface for controlling the system and monitoring the deposition process.

**Operational Procedure:**

1.  Load desired image pattern into the control software.
2.  Select appropriate fluids and surfactants for the emulsion.
3.  Configure micro-pump rates and mixing parameters.
4.  Load support plate onto the XY stage.
5.  Initiate deposition process. The control software will generate nozzle trajectories and control the micro-pumps to deposit the emulsion onto the support plate according to the image pattern.
6.  Monitor the deposition process using the real-time feedback system.
7.  Once the deposition is complete, remove the support plate.

**Potential Applications:**

*   Reconfigurable displays with dynamic contrast and color.
*   Adaptive optics with locally adjustable refractive index.
*   Microfluidic devices with on-demand fluid control.
*   Biomedical sensors with localized chemical gradients.