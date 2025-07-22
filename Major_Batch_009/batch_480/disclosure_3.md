# 10227169

## Automated Liner Folding & Stacking System

**Concept:** A robotic system integrated with the liner manufacturing process to automatically fold, stack, and sleeve liners into pre-formed, transparent bags. This addresses potential bottlenecks in packaging, minimizes manual labor, and increases throughput.

**Specifications:**

1.  **Liner Production Integration:** System designed to interface directly with existing paperboard liner production lines.  Accepts flat, scored liners as input.
2.  **Robotic Arm(s):** Two (2) six-axis robotic arms with specialized end-effectors.
    *   **End-Effector 1 (Folding/Insertion):**  Vacuum grippers paired with small articulating ‘fingers’ to precisely fold the bottom panel and tuck the tab.  Force sensors to prevent damage to the paperboard.
    *   **End-Effector 2 (Bag Handling/Placement):**  Vacuum gripper designed to securely hold and position the transparent bag. Includes a small ‘bell’ shape to create a temporary opening in the bag.
3.  **Vision System:** High-resolution camera system with integrated image processing.
    *   **Functionality:**  Liner detection, alignment, score line verification, bag opening detection, liner presence verification within the bag.
    *   **Lighting:**  Backlighting and diffuse overhead lighting to maximize contrast.
4.  **Bag Magazine:** Automated magazine capable of holding 1000+ transparent bags.  Bag separation mechanism utilizing vacuum and gentle pushers.
5.  **Conveyor System:**  Low-friction conveyor belt to move liners and sleeved bags through the system.  Variable speed control.
6.  **Control System:**  PLC-based control system with HMI touchscreen interface.
    *   **Features:**  Adjustable folding parameters (fold angle, tuck depth), bag feed rate control, error detection and logging, cycle time monitoring.
7.  **Stacking Mechanism:**  Automated stacking mechanism that receives sleeved liners and stacks them in pre-defined layers.
    *   **Stacking Configuration:** Programmable layer height and stack orientation.
    *   **Output:** Stacked liners are discharged onto a conveyor for further processing or packaging.
8.  **Safety Features:**  Light curtains, emergency stop buttons, and interlocked safety gates.
9.  **System Throughput:** Target throughput of 120 liners per minute.
10. **Pseudocode (Folding Subroutine):**

```
FUNCTION FoldLiner(liner_object)
    // Input: Liner object with defined score lines and dimensions
    
    ALIGN liner_object to conveyor position

    ACTIVATE vacuum grippers on End-Effector 1

    FOLD bottom panel along first score line (angle = 90 degrees)
    
    ACTIVATE force sensors to monitor fold resistance
    
    FOLD folding tab along second score line (angle = 90 degrees)
    
    INSERT folding tab between front panel and back panel 
    
    VERIFY secure closure with vision system 
    
    RELEASE vacuum grippers
    
    RETURN success/failure status
END FUNCTION
```

**Innovation:** This system moves beyond simply containing the food product, it automates the *creation* of the container itself, inline with production. This is a significant step towards fully automated food packaging. Furthermore, the integration of a vision system ensures quality control at each step, minimizing waste and maximizing efficiency. This also allows for easy adaptation to different liner/bag sizes with minimal re-tooling.