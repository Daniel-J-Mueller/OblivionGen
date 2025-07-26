# 9581804

## Microfluidic Electrowetting Array for Dynamic Droplet Manipulation & Biofilm Analysis

**Concept:** Leverage the liquid dispensing method described in the patent to create a high-density, dynamically reconfigurable microfluidic array for manipulating individual droplets and, crucially, for real-time analysis of biofilm formation and disruption.

**Specs:**

*   **Array Dimensions:** 1024 x 1024 individually addressable micro-wells. Each well: 50μm diameter, 25μm depth.
*   **Support Plate Material:**  Borosilicate glass with integrated ITO (Indium Tin Oxide) electrodes beneath each micro-well.  Electrode pitch: 50μm.
*   **First Liquid (Dispersed Phase):** A biocompatible, conductive oil (e.g., mineral oil doped with tetraalkylammonium salt) - acts as the carrier fluid for individual droplets. Low surface tension for droplet formation.
*   **Second Liquid (Continuous Phase):** Aqueous buffer solution (PBS, LB, etc.) optimized for microbial growth or assay chemistry.  
*   **First Portion (Solute):** A mixture of surfactants (e.g., Tween-20, Triton X-100) and a fluorescent dye (e.g., SYTO 9) for live/dead bacterial staining. Concentration optimized for biofilm visualization and quantification. This is the 'active' component transferred to the continuous phase.
*   **Dispensing Method:**  Slit-filling method modified for microfluidic scale. Precisely controlled dispensing volumes (<1 nL per well) using piezoelectric actuators. 
*   **Control System:** Custom software and hardware for individually addressing electrodes, applying voltages, and controlling dispensing actuators.  
*   **Imaging System:** High-resolution fluorescence microscope with automated stage control for scanning the entire array.
*   **Data Analysis:** Algorithms for automated biofilm quantification (biomass, roughness, cell viability) based on fluorescence intensity and image processing.

**Operational Procedure:**

1.  **Array Preparation:** Support plate is cleaned and functionalized for optimal wetting and droplet formation.
2.  **Initial Filling:**  The micro-wells are initially filled with the first liquid (conductive oil) using slit-filling.
3.  **Droplet Generation:** Aqueous droplets containing bacteria or assay reagents are dispensed into the first liquid within each micro-well.
4.  **Surfactant Transfer & Biofilm Formation:** A defined volume of the first portion (surfactant/dye mix) is dispensed onto the conductive oil, initiating transfer into the surrounding aqueous droplet. This process ensures uniform distribution of the surfactant/dye within the aqueous phase. Bacteria are allowed to attach and form biofilms on the micro-well surface.
5.  **Dynamic Manipulation:** Individual electrodes are selectively activated, inducing electrowetting and allowing for precise movement, merging, and splitting of droplets within the array.
6.  **Real-Time Monitoring:** Fluorescence microscopy is used to monitor biofilm formation, assess cell viability, and track the effects of antimicrobial agents or other treatments.
7.  **Automated Analysis:** Image processing algorithms quantify biofilm characteristics and generate data for analysis.

**Pseudocode (Control Sequence - Single Well):**

```
// Initialize: Set ITO voltage to 0V
ITO_Voltage = 0;

// Dispense First Liquid (Conductive Oil)
Dispense_Volume(Well_ID, First_Liquid, 100 nL);

// Dispense Second Liquid (Aqueous Droplet with Bacteria)
Dispense_Volume(Well_ID, Second_Liquid, 20 nL);

//Dispense First Portion (Surfactant/Dye)
Dispense_Volume(Well_ID, First_Portion, 2 nL);

//Apply Voltage to initiate surfactant transfer & biofilm formation
ITO_Voltage = 5V;  //Adjust voltage for optimal transfer
Wait(60 seconds);

//Imaging
Capture_Image(Well_ID, Fluorescence);

//Data Analysis
Biofilm_Biomass = Analyze_Image(Fluorescence, "Biomass");
Cell_Viability = Analyze_Image(Fluorescence, "Cell Viability");

//Repeat imaging & analysis at defined time intervals

//Move droplet
ITO_Voltage = 10V;
Wait(5 seconds);
ITO_Voltage = 0;
```

**Novelty:** The high-density microfluidic array coupled with dynamic electrowetting control provides a unique platform for studying biofilm formation and disruption in a spatially resolved manner. This allows for the investigation of microbial behavior at the single-cell level and facilitates the development of novel antimicrobial strategies.