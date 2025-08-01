# 9867482

**Modular Vertical Garden System with Integrated Container Locking**

**Concept:** Expand the interlocking container concept into a full vertical garden system. Instead of just shelf-based interlocking, containers will interlock *vertically* as well, creating a stable, customizable wall-mounted garden.

**Specs:**

*   **Container Modules:**
    *   Dimensions: 30cm x 30cm x 20cm (Standard size, configurable).
    *   Material: Recycled polypropylene, UV resistant.
    *   Construction: Open-top container with reinforced corners. Drainage holes at the base.
    *   Locking Mechanism:  Each container has four integrated locking tabs – one on each vertical side. These tabs are angled inwards. Corresponding slots are on the exterior walls of adjacent containers.
    *   Vertical Interlock Strength:  Each tab/slot pair can withstand a static load of 5kg.
    *   Color Options: Dark Green, Terracotta, Charcoal.
*   **Wall Mounting Brackets:**
    *   Material: Powder-coated steel.
    *   Attachment:  Secure to wall studs via screws.
    *   Bracket Type:  ‘Ladder’ style, with horizontal rungs spaced to accept the bottom of the containers.
    *   Load Capacity:  Each bracket supports up to 20kg.
*   **Horizontal Stabilizer Bars:**
    *   Material: Aluminum alloy.
    *   Function:  Clip onto the sides of multiple vertically stacked containers, providing lateral stability.
    *   Attachment:  Spring-loaded clips that snap onto the container edges.
*   **Watering System Integration:**
    *   Optional:  A drip irrigation system can be integrated, with tubing running along the top row of containers and drippers inserted into each container.
    *   Reservoir: A wall-mounted reservoir with a pump can supply water to the system.
*   **Container Varieties:**
    *   Standard: For general planting.
    *   Herb/Strawberry: Shallow containers with multiple planting slots.
    *   Trailing: Containers with openings on the sides for cascading plants.

**Pseudocode (Vertical Stacking Logic):**

```
function stackContainers(container1, container2):
  if (container1.bottomAlignsWith(container2.top)):
    container2.slotAccepts(container1.tab)
    container1.locksTo(container2)
    return success
  else:
    return failure
```

**Innovation Highlights:**

*   **Scalability:** The modular design allows users to create gardens of any size or shape.
*   **Customization:** Variety of container types and colors cater to different gardening needs and aesthetic preferences.
*   **Stability:** Vertical interlocking and horizontal stabilizer bars ensure a strong and secure structure.
*   **Integrated Watering:** Optional drip irrigation simplifies plant care and conserves water.
*   **Urban Gardening Solution:**  Provides a space-saving way to grow plants in urban environments.