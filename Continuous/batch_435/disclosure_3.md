# 10227169

## Modular, Stackable Food Container System

**Concept:** Expand the single-item container to a fully customizable, stackable, and visually appealing system for multiple food items and portion control. Leverage the liner's structural properties for modularity.

**Specs:**

*   **Base Liner Material:** Same as current patent - 18pt paperboard.
*   **Liner Modules:**
    *   **Single Item Liner:** Existing design. Dimensions as specified in claim 13 (2" x 5.25" x 5.25" x .625").
    *   **Half-Module Liner:** 2" x 2.625" x 5.25" x .625".  Designed to slot alongside another Half-Module or a Single Item Liner.
    *   **Quarter-Module Liner:** 2" x 1.3125" x 5.25" x .625". Designed to slot alongside other Quarter/Half/Single Item Liners.
    *   **All modules share the same height (2")** and the same basic folding score pattern (as claim 1) for consistent stackability.
*   **Stacking Features:**
    *   The top edge of each liner module will incorporate a small, integrated lip/ridge formed during the folding process.
    *   This lip will engage with corresponding recesses molded into the inside of the bag (see below) to provide vertical stability when stacked.
*   **Bag Material:** Thicker, more rigid cellophane (approximately 5 mil) to support stacking and maintain visibility.
*   **Bag Design:**
    *   **Modular Bag:** A single, large cellophane bag designed to encompass multiple liner modules.
    *   **Internal Recesses:** The inside of the bag will be thermoformed with corresponding recesses for the liner module lips, securing the modules and preventing shifting during transport. The modular bag will allow the user to slide liners in and out as desired.
    *   **Closure System:** A zip-lock style closure across the top of the bag to securely seal the contents.
    *   **Reinforced Base:** A rigid cardboard insert at the base of the bag to provide a flat, stable base.
*   **Display/Presentation Features:**
    *   **Clear Window:** A large, clear window on the front of the bag to showcase the food items.
    *   **Labeling Area:** A designated area on the bag for custom labels or branding.
*   **Assembly:**
    *   Users select desired liner modules.
    *   Modules are placed within the modular bag.
    *   Bag is sealed.

**Pseudocode for Thermoforming Process:**

```
FUNCTION ThermoformBag(moduleDimensions, recessDepth)
  // moduleDimensions: Array of dimensions for each module type
  // recessDepth: Depth of recesses in the bag

  FOR EACH moduleType IN moduleType
    CREATE moldSection(moduleType.width, moduleType.length, recessDepth)
    ATTACH moldSection TO bagInterior
  END FOR

  APPLY heat AND vacuum TO shape bag TO mold
END FUNCTION
```

**Variations:**

*   Different module sizes and shapes to accommodate various food items.
*   Insulated bag materials for temperature-sensitive foods.
*   Reusable bag options.
*   Modular tray inserts for additional support.