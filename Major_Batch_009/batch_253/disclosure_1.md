# 10067918

## Dynamic Text Unit Compositing & Behavioral Profiles

**Concept:** Extend the core concept of bound/unbound text units by allowing composite units formed from multiple smaller, initially unbound text segments. These composites gain behavioral profiles – rules dictating how they react to selection, modification, and external triggers. This enables a richer, more interactive document experience beyond simple selection/highlighting.

**Specs:**

**1. Data Structures:**

*   `TextSegment`: Basic unit – character string with start/end index within the document. Stores formatting (font, color, size, etc.).
*   `TextUnit`:  Array of `TextSegment` pointers.  Flags for 'bound' (default), 'composite', 'active'.
*   `BehavioralProfile`:
    *   `trigger`: Condition for profile activation (e.g., user interaction, timer, external event).
    *   `selectionBehavior`: Defines what happens on selection (highlight, expand, collapse, execute action). Enum: `HIGHLIGHT`, `EXPAND`, `COLLAPSE`, `EXECUTE_ACTION`.
    *   `modificationBehavior`:  Defines behavior during editing (prevent edit, auto-correct, trigger action). Enum: `PREVENT_EDIT`, `AUTO_CORRECT`, `TRIGGER_ACTION`.
    *   `action`:  Pointer to a function/script to execute on action trigger.  Accepts `TextUnit` as input.
*   `Document`: Contains array of `TextUnit`s, mapping to the document content.

**2. Core Functions:**

*   `CreateTextUnit(start, end)`: Creates a `TextUnit` from a specified range of characters.  Defaults to 'bound'.
*   `CreateCompositeUnit(unit1, unit2, ...)`:  Creates a `TextUnit` that's a composite of other units. All constituent units become “unbound” and are managed by the composite unit.
*   `AssignBehaviorProfile(unit, profile)`: Assigns a `BehavioralProfile` to a `TextUnit`.
*   `HandleSelection(selectedRange)`:
    *   Iterates through `TextUnit`s to find overlapping units.
    *   For each overlapping unit:
        *   Check `BehavioralProfile`.
        *   Apply `selectionBehavior`.
        *   If `EXECUTE_ACTION`, call the linked function.
*   `HandleModification(start, end, newText)`:
    *   Identifies affected `TextUnit`s.
    *   Checks `BehavioralProfile`.
    *   Applies `modificationBehavior`.
    *   Updates document content accordingly.

**3. Pseudocode – Selection Handling:**

```
function HandleSelection(selectedRange) {
  overlappingUnits = FindOverlappingTextUnits(selectedRange)

  for each unit in overlappingUnits {
    if (unit.isComposite) {
      // Composite unit – iterate through constituent units.
      for each constituentUnit in unit.ConstituentUnits {
        ApplyBehavior(constituentUnit, selectedRange)
      }
    } else {
      ApplyBehavior(unit, selectedRange)
    }
  }
}

function ApplyBehavior(unit, selectedRange) {
  if (unit.BehaviorProfile != null) {
    switch (unit.BehaviorProfile.selectionBehavior) {
      case HIGHLIGHT:
        HighlightText(unit);
      case EXPAND:
        ExpandSelection(unit); // Expand selection to cover the entire unit.
      case COLLAPSE:
        CollapseSelection(unit); // Reduce selection to a single character.
      case EXECUTE_ACTION:
        ExecuteAction(unit, selectedRange); // Call the linked function.
    }
  } else {
    // Default behavior: Highlight.
    HighlightText(unit);
  }
}
```

**4. Expansion – Example Use Cases:**

*   **Interactive Forms:** Composite units representing form fields.  Selection triggers input validation or auto-completion.
*   **Dynamic Summarization:**  Selection of a paragraph expands to show a concise summary.
*   **Cross-Referencing:** Selecting a term expands to show all related references in the document.
*   **Code Editors:**  Selection of a variable expands to show all usages or definitions.