# D902992

## Modular Gift Card Holder System - "Card Bloom"

**Concept:** A gift card holder that isn't a *holder* in the traditional sense, but a modular system allowing for dynamic display and expansion. Inspired by origami/kirigami principles, but using magnetically connected, thin panels.

**Core Components:**

*   **Base Unit:** A small, flat, magnetically receptive base. Dimensions: 6cm x 4cm x 0.5cm. Material: Recycled ABS plastic with embedded ferrite for magnetic coupling.
*   **Panel Modules:** Thin, petal-shaped panels (~5cm x 3cm, 1mm thick).  Material:  Flexible, bio-degradable PLA coated with a durable, matte finish. Each panel has embedded magnets along one edge for connection to the base and other panels. Multiple color/texture options.
*   **Card Slot Module:**  A slightly thicker panel (~5cm x 3cm, 2mm thick) with a laser-cut slot to securely hold a standard-size gift card.  Magnetically connects to other panels.
*   **Connector Modules:** Small, magnetically receptive "links" allowing for variable angles and connections between panels. Useful for creating more complex configurations.
*   **Display Stand Module:** A magnetically-attached, foldable stand to allow the "bloom" to be displayed upright.

**Functionality:**

1.  The user begins with the base unit.
2.  They insert the gift card into the Card Slot Module.
3.  The user then magnetically attaches Panel Modules around the Card Slot Module to the base, creating a "bloom" effect.  The number of panels is variable.
4.  Connector Modules can be used to create angles and curves, making each bloom unique.
5.  The Display Stand Module can be attached to elevate the bloom.

**Pseudocode (Assembly Logic - conceptual, for manufacturing):**

```
FUNCTION assembleBloom(baseUnit, cardSlotModule, panelList, connectorList, displayStandModule):

  // 1. Attach Card Slot to Base
  attachMagnetically(cardSlotModule, baseUnit)

  // 2. Attach Panels - user defined order
  FOR EACH panel IN panelList:
    attachMagnetically(panel, baseUnit OR lastAttachedPanel OR cardSlotModule)

  // 3. Attach Connectors (optional)
  FOR EACH connector IN connectorList:
    attachMagnetically(connector, baseUnit OR lastAttachedPanel OR cardSlotModule)

  // 4. Attach Display Stand (optional)
  IF displayStandModule != NULL:
    attachMagnetically(displayStandModule, baseUnit)

  RETURN assembledBloom
```

**Manufacturing Notes:**

*   Panels could be mass-produced using injection molding or laser cutting.
*   Magnet placement must be precise for secure connections.
*   PLA coating needs to be durable and scratch-resistant.
*   The system is designed to be customizable and expandable, with new panel designs and accessories.
*   Consider incorporating NFC tags within the panels for digital gifting options.