# 9581804

## Microfluidic Electrowetting Array for High-Throughput Biological Screening

**Concept:** Leverage the described liquid dispensing method to create a massively parallel microfluidic device for screening biological samples. Instead of focusing on single electrowetting device function, the aim is to create a dense array where each individual droplet functions as a micro-reactor or sensing unit.

**Specs:**

*   **Substrate:** Silicon wafer with integrated micro-channels. Channels will be 50-100 microns wide, spaced 25-50 microns apart. Hydrophobic coating (e.g., silanization) applied to channel surfaces.
*   **Electrode Array:**  Patterned ITO (Indium Tin Oxide) electrodes deposited beneath the micro-channels. Electrode pitch matches channel spacing.  Each electrode individually addressable via multiplexing circuitry. Electrode size: 25-50 microns diameter.
*   **First Liquid (Dispersed Phase):** Aqueous solution containing biological sample (cells, proteins, DNA).  Additives to control surface tension and droplet stability.  Fluorescent reporters for signal detection.
*   **Second Liquid (Continuous Phase):**  Oil-based fluid (e.g., fluorinated oil) immiscible with aqueous phase. Optimized viscosity for droplet formation and stability. Dissolved oxygen for cell viability (if applicable).
*   **Droplet Generation:**  Microfluidic chip design incorporates droplet generation junctions.  Flow rates of aqueous and oil phases precisely controlled to create monodisperse droplets. Droplet size: 10-50 microns diameter.
*   **Reagent Delivery:** Microfluidic channels integrated for delivering reagents directly into droplets. Reagent delivery controlled via micro-pumps and micro-valves.
*   **Detection System:**  High-resolution fluorescence microscope with automated scanning capability.  Image analysis software to quantify fluorescence signals within droplets.
*   **Array Size:**  Initial prototype: 100x100 array (10,000 droplets). Scalable to larger arrays (e.g., 1000x1000).
*   **Control System:**  Custom software to control all aspects of the device: fluid flow rates, electrode voltages, image acquisition, data analysis.

**Operational Sequence (Pseudocode):**

```
Initialize System:
    Load chip with first & second liquids
    Initialize fluid flow rates
    Initialize electrode control system
    Initialize detection system

For Each Droplet in Array:
    Dispense first liquid (sample) into second liquid (oil) at droplet generation junction.
    Apply voltage to electrode beneath droplet to control droplet shape and movement.
    Deliver reagents to droplet via microfluidic channels.
    Incubate droplet for specified time.
    Acquire fluorescence image of droplet.
    Analyze image to quantify signal.
    Store data.

Repeat For Next Droplet.

Output:
    Data array containing signal intensity for each droplet.
```

**Innovation:** Utilizing the precise liquid dispensing as a method for droplet microfluidics, enabling massively parallel biological assays. This moves beyond individual device control to a high-throughput screening platform, capable of analyzing thousands of samples simultaneously. Scalability and automation minimize human intervention and maximize data throughput. The precise control over droplet formation and manipulation, coupled with integrated reagent delivery, enables complex biological experiments within a microfluidic environment.