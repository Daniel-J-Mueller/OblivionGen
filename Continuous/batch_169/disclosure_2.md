# 9514925

## Automated Micro-Fluidic Die Coating & Inspection System

**Concept:** Instead of a static coating application post-dicing, integrate a micro-fluidic coating system *during* the dicing process. This allows for precise, localized coating application directly into the saw streets *as* the die are being separated, and immediate post-application inspection.

**System Components:**

1.  **Modified Dicing Saw:** The standard dicing saw is outfitted with multiple micro-fluidic nozzles positioned *adjacent* to the blade. These nozzles are connected to a reservoir of the epoxy adhesive/coating material.
2.  **Micro-Fluidic Control System:** A high-precision pump and valve system regulates the flow of coating material to each nozzle. This allows for variable coating thickness and composition, tailored to specific die designs or requirements.
3.  **Vision System (Integrated):** High-resolution cameras, positioned *above and below* the wafer, monitor the coating process in real-time. This system performs the following functions:
    *   **Street Detection:** Identifies the saw streets based on the cutting path of the blade.
    *   **Coating Monitoring:**  Monitors the volume and distribution of the coating material within the streets. Detects voids, bubbles, or unevenness.
    *   **Post-Coating Inspection:**  Inspects the coated die for any defects (e.g., coating spills, incomplete coverage) *immediately after* separation.
4.  **Automated Wafer Handling System:**  A robotic system loads, positions, and removes wafers from the dicing/coating station.
5.  **Closed-Loop Control System:** The vision system provides feedback to the micro-fluidic control system, adjusting the coating flow to ensure optimal coverage and quality.

**Operational Procedure:**

1.  Wafer is loaded onto the dicing/coating station.
2.  Dicing blade and micro-fluidic nozzles are aligned with the first saw street.
3.  Blade initiates cutting. Simultaneously, the micro-fluidic nozzles dispense the coating material into the saw street *ahead* of the blade.
4.  The closed-loop control system adjusts the coating flow based on the vision system’s feedback.
5.  The process repeats for each saw street.
6.  Once all die are separated, the vision system performs a final inspection.
7.  The separated die are automatically transferred to the next stage of the manufacturing process.

**Pseudocode (Control System Logic):**

```
// Variables
streetWidth = // measured from wafer map
bladeSpeed = //current blade speed
coatingFlowRate = //initial flow rate
visionData = //data from the vision system

// Main Loop (executed for each saw street)
while (street not fully cut):
    coatingFlowRate = initialFlowRate * (bladeSpeed / targetBladeSpeed)
    dispenseCoating(coatingFlowRate, streetWidth)
    visionData = captureVisionData(street)
    if (visionData.voidDetected == True):
        increaseCoatingFlowRate(coatingFlowRate * 1.1) //adjust flow
    if (visionData.spillDetected == True):
        decreaseCoatingFlowRate(coatingFlowRate * 0.9)
    if (visionData.coverageIncomplete == True):
        increaseCoatingFlowRate(coatingFlowRate * 1.05)
    updateCoatingFlowRate(coatingFlowRate)
    wait(timeStep)

```

**Material Specifications:**

*   Coating Material: UV-curable epoxy adhesive with optimized viscosity for micro-fluidic dispensing.
*   Nozzle Material: Chemically resistant polymer (e.g., PTFE) with precise micro-machined orifice.

**Potential Benefits:**

*   Reduced Coating Defects: Precise, localized coating minimizes spills and voids.
*   Improved Throughput: Coating and dicing are performed simultaneously.
*   Enhanced Process Control: Real-time inspection and feedback.
*   Reduced Material Waste: Optimized coating application.