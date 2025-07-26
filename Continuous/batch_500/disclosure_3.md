# 10726469

## Dynamic Attribute ‘Recipes’ & Proactive Substitution

**Concept:** Expand beyond simple attribute suggestions based on *past purchases* to allow users to define ‘recipes’ for item attributes, and proactively offer substitutions based on those recipes *before* an item is added to the cart, or when stock changes.

**Specs:**

**1. User-Defined Attribute Recipes:**

*   **Interface:** Within the user account settings, a dedicated “Attribute Recipes” section.
*   **Recipe Creation:** Users can define recipes based on item categories (e.g., “Coffee,” “Running Shoes,” “Office Chair”).
*   **Recipe Structure:** Each recipe is a set of prioritized attribute preferences.  Example:
    *   Category: “Coffee”
    *   Priority 1:  Roast Level = “Dark”
    *   Priority 2:  Origin = “Sumatra”
    *   Priority 3:  Fair Trade = “Yes”
    *   Priority 4:  Grind = "Coarse"
*   **Recipe Naming:** Users assign a name to each recipe (e.g., “My Daily Brew,” “Weekend Espresso”).
*   **Recipe Activation:** Users can activate or deactivate recipes.

**2. Proactive Attribute Suggestion Engine:**

*   **Pre-Cart Interaction:** As the user browses items, the engine analyzes the current item's category.
*   **Recipe Matching:** The engine identifies active recipes that match the item's category.
*   **Attribute Comparison:** The engine compares the item's current attributes to the prioritized attributes in the matched recipe.
*   **Proactive Suggestion Display:** *Before* the user adds the item to the cart, a subtle notification appears, displaying potential attribute substitutions to better align with the user’s recipe.  Example:  “Based on your ‘My Daily Brew’ recipe, consider a Dark Roast instead of Medium Roast.” The notification displays the preferred attribute value *and* the current value.

**3. Dynamic Substitution on Stock Changes:**

*   **Real-time Stock Monitoring:** The system continuously monitors item stock levels.
*   **Out-of-Stock Detection:** When an item with a user’s preferred attribute becomes out of stock, the system triggers a substitution suggestion.
*   **Substitution Display:**  Within the cart (or on the product page), a prominent notification displays the out-of-stock item *and* a suggested substitute based on the user's recipes.  Example:  “Dark Roast Coffee is currently unavailable.  We suggest a Sumatra blend, aligning with your ‘My Daily Brew’ recipe.”
*   **Automatic Substitution (Optional):**  A user setting to *automatically* substitute items with preferred attributes if the original is unavailable.  Confirmation prompt required.

**4.  Algorithm Pseudocode:**

```
FUNCTION suggest_attribute_substitutions(user, item, active_recipes)
  // 1. Find matching recipes
  matching_recipes = SELECT recipes FROM active_recipes WHERE recipe.category == item.category

  IF matching_recipes IS EMPTY THEN
    RETURN // No recipes match - no suggestions

  // 2. Sort recipes by number of matching attributes
  sorted_recipes = SORT matching_recipes BY (COUNT of attributes in recipe matching item) DESC

  // 3.  Get the top recipe
  top_recipe = sorted_recipes[0]

  // 4.  Identify attribute differences
  attribute_differences = FIND attributes in top_recipe that are DIFFERENT from item attributes

  // 5.  Create substitution suggestions
  suggestions = []
  FOR each difference IN attribute_differences
    suggestion = {
      attribute: difference.attribute,
      current_value: item[difference.attribute],
      suggested_value: top_recipe[difference.attribute]
    }
    APPEND suggestion TO suggestions

  RETURN suggestions
```

**5. UI Considerations:**

*   Subtle notification styling to avoid disrupting browsing.
*   Clear explanation of *why* a substitution is being suggested (based on the user’s recipe).
*   Easy ability to dismiss suggestions or customize recipe preferences.
*   Visual highlighting of suggested substitutions within the cart or product details.