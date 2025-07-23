# 10227169

**Automated Liner & Bag Forming System – “FlowWrap”**

**Concept:** An inline system for simultaneously forming the paperboard liner and applying/sealing the plastic bag, integrated into existing bakery/food production lines. This eliminates manual liner/bag insertion, increasing throughput and reducing labor costs.

**System Components:**

1.  **Paperboard Roll Feed:** Continuous feed of pre-printed, pre-scored paperboard. Scoring replicates the existing liner folds.
2.  **Forming Station:** A series of rotating dies and guides to fold and shape the paperboard into the fully formed liner. Uses servo motors for precise positioning and timing.
3.  **Bag Film Feed:** Roll of transparent plastic film.  Similar to standard flow-wrap machinery.
4.  **Encasement/Sealing Station:** The formed liner is conveyed into the plastic film.  A rotating sealing jaw creates a fin seal or lap seal on both ends of the bag, encasing the liner.
5.  **Product Detection/Indexing:** Sensors detect the presence of a food product on the conveyor.  The system indexes to ensure proper product placement within the formed liner *before* bag encasement.
6.  **Output Conveyor:** Delivers sealed containers to packaging/labeling stations.
7.  **PLC Control System:**  Manages all system functions, monitors sensors, and adjusts parameters (speed, temperature, seal pressure) based on product type and desired output.

**Pseudocode – PLC Logic (Simplified):**

```
LOOP
    IF ProductDetected THEN
        Activate Liner Forming Station
        Activate Bag Film Feed
        Position Liner within Bag Film
        Activate Sealing Station
        Seal Bag – (Temperature = X, Pressure = Y, Duration = Z)
        Increment Container Count
        Output Container to Conveyor
    ENDIF
    Monitor Sensors (Product, Film, Errors)
    Adjust Parameters (Speed, Temperature) based on Input
ENDLOOP
```

**Specifications:**

*   **Throughput:** Target 120+ containers per minute.
*   **Container Size Range:** Accommodate liners/bags from 3” x 3” to 6” x 6” (adjustable dies).
*   **Film Thickness:** 0.5 mil – 2 mil plastic film.
*   **Paperboard Thickness:** 18pt – 24pt paperboard.
*   **Sealing Type:** Fin seal or lap seal (selectable).
*   **Safety Features:** Emergency stop buttons, safety guards, interlocks.
*   **HMI:** Touchscreen interface for parameter adjustment and status monitoring.

**Refinements:**

*   **Automated Label Applicator:** Integrate a label applicator to apply adhesive labels directly to the sealed bag.
*   **Vision System:** Implement a vision system to inspect the sealed container for defects (seal integrity, label placement).
*   **Recipe Management:** Store pre-defined recipes for different product types and container sizes.
*   **Remote Monitoring:** Enable remote monitoring and control via a web interface.
*   **Integration with robotic pick and place systems:** Automatically place the baked good into the newly formed container.