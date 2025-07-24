# 10507949

## Modular, Stackable Food Container System

**Concept:** Expand the single-piece container to a modular system allowing stacking and reconfiguration for multi-course meals or family-style service. Utilize integrated, flexible locking mechanisms formed from the cardboard itself, eliminating the need for separate fasteners.

**Specs:**

*   **Base Unit:** Dimensions mirroring the existing patent’s unfolded plate size (approximately 8" x 12"). Constructed from a heavier grade cardboard (24pt) with a grease-resistant coating on both sides.
*   **Modular Inserts:** Three insert types, all designed to fit *within* the base unit, creating tiered levels:
    *   **Shallow Tray (ST):** 0.75” deep. For appetizers, salads, or small sides. Quantity per stack: unlimited
    *   **Deep Well (DW):** 2” deep. For entrees, main courses, or pasta dishes. Quantity per stack: 2 max.
    *   **Compartment Divider (CD):**  Creates two equal-sized compartments within the base unit or a DW insert. Quantity per stack: 1 max.
*   **Locking Mechanism:** Each insert (ST, DW, CD) has four “flex tabs” – precisely creased sections of cardboard extending from the side walls. These tabs are designed to *snap* into corresponding slots pre-cut into the side walls of the base unit and other inserts *below* them. 
    *   Slot dimensions: 0.25” x 0.5”.
    *   Flex Tab dimensions: 0.25" wide, 0.4" long.
    *   Crease angle: 45 degrees (allows for flex and secure locking).
*   **Stacking Height:** Maximum stack height: 6” (allows for stable transport and service).
*   **Lid Option:** A domed, transparent (biodegradable plastic or coated cardboard) lid that snaps onto the top of the stack. Lid incorporates a locking tab for secure closure.
*   **Fold Pattern Adaptation:** The existing patent’s fold patterns are adapted to create the side walls of the inserts. The bottom of each insert is a solid panel for structural integrity.
*   **Assembly:** Inserts are assembled manually by snapping the flex tabs into the corresponding slots. A small amount of pressure may be required.

**Pseudocode (Insert Creation Logic):**

```
FUNCTION CreateInsert(insertType, dimensions)
  // insertType: "ShallowTray", "DeepWell", "CompartmentDivider"
  // dimensions: width, length, height

  CreateBasePanel(width, length) // Flat cardboard panel
  IF insertType == "CompartmentDivider" THEN
    CreateDividerPanel(height, width) // Vertical panel to bisect the space
  ENDIF

  FoldSideWalls(height) // Fold up side walls according to crease pattern
  CutSlotOpenings() // Cut slots for flex tab insertion
  CreateFlexTabs() // Create flex tabs on side walls
  ApplyGreaseResistantCoating()

  RETURN Insert
ENDFUNCTION
```

**Material Specifications:**

*   Cardboard: Recycled, unbleached cardboard (24pt for base unit, 18pt for inserts).
*   Coating: Water-based, biodegradable grease-resistant coating.
*   Lid (Optional): Biodegradable PLA plastic or coated cardboard.

**Potential Applications:**

*   Food delivery and takeout containers.
*   Catering and event service.
*   Picnics and outdoor gatherings.
*   Meal prepping and portion control.