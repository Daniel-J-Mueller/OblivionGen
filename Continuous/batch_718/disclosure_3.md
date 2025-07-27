# 7930220

## Dynamic Contextual Item Grouping & ‘Wishlist Preview’

**Concept:** Expand the drag-and-drop functionality to *group* items based on contextual similarity *before* assignment to shipping addresses. Simultaneously, introduce a 'Wishlist Preview' dynamically overlaid on the checkout page to visualize potential future purchases impacting shipping costs.

**Specification:**

**I. Data Structures:**

*   `ItemContext` (Object): Represents contextual data for each item.
    *   `category` (String):  Broad category (e.g., "Electronics", "Clothing").
    *   `subCategory` (String): More specific grouping (e.g., "Headphones", "T-Shirts").
    *   `relatedItems` (Array of Item IDs):  Items frequently purchased together or algorithmically similar.
    *   `urgency` (Integer, 1-5):  Determined by purchase history/seasonal demand (High=1, Low=5).
*   `ShippingProfile` (Object): Defines shipping cost and options based on destination, weight, and *item context groups*.
    *   `destinationAddress` (String)
    *   `totalWeight` (Float)
    *   `contextGroups` (Array of ContextGroup IDs)
    *   `estimatedCost` (Float)

**II. System Components:**

1.  **Contextual Analysis Engine:**  A server-side module.
    *   Receives item data (IDs, attributes).
    *   Populates `ItemContext` objects using historical data, product catalogs, and machine learning (recommendation engine).
    *   Exposes an API to retrieve `ItemContext` data for an item ID.
2.  **Drag & Drop Grouping Interface:** Client-side (JavaScript).
    *   Extends existing drag-and-drop functionality.
    *   Allows user to drag items *onto* dynamically generated "context groups" displayed on the checkout page.  These groups are visually distinct and labeled (e.g., "Electronics – High Urgency", "Gifts for Dad").
    *   When an item is dragged over a context group, visual cues indicate compatibility (e.g., highlight, animation).
    *   Groups can be collapsed/expanded for easier management.
3.  **Wishlist Preview Module:**  Client-side (JavaScript).
    *   Accesses user’s saved wishlist data.
    *   Dynamically overlays a semi-transparent preview of wishlist items *onto* the checkout page.
    *   Displays estimated shipping cost *if* those wishlist items were added to the current order.
    *   Allows user to seamlessly add wishlist items to the current order.
4.  **Shipping Cost Calculator:** Server-side module.
    *   Receives a list of `ShippingProfile` objects (representing item groupings).
    *   Calculates shipping costs for each grouping based on predefined rules, carrier rates, and weight.
    *   Returns optimized shipping options to the client.

**III.  Pseudocode (Client-Side - Drag & Drop)**

```javascript
//On page load
populateContextGroups();

function populateContextGroups(){
    //Fetch item context data for all items in cart
    //Create dynamic context groups based on fetched data
    //Display context groups on page
}

function handleItemDrag(itemID) {
    //On drag start, store itemID

    //On drag over context group
    if (isCompatible(itemID, contextGroupID)) {
        //Highlight context group
    } else {
        //Dim context group
    }
}

function handleItemDrop(itemID, contextGroupID) {
    //Assign item to context group
    //Update visual representation
    //Recalculate shipping costs
}
```

**IV. User Flow:**

1.  User adds items to cart.
2.  Checkout page loads, displaying items and dynamically generated context groups.
3.  User drags items onto relevant context groups.
4.  Wishlist Preview displays estimated shipping costs if wishlist items are added.
5.  User confirms groupings and proceeds to payment.
6.  System calculates final shipping costs based on grouped items and shipping profiles.