# 10067918

## Dynamic Text Unit Composition & Behavioral Tagging

**Concept:** Extend the idea of bound/unbound text units to allow *dynamic* composition of units based on user interaction *and* attach behavioral tags to dictate how these units respond to selection/manipulation. This goes beyond simple binding/unbinding; it creates “living” text segments.

**Specs:**

**1. Core Data Structure: Behavioral Text Unit (BTU)**

*   `text_segment`: String – The actual text content.
*   `unit_id`: UUID – Unique identifier for the unit.
*   `composition_rules`: List of Rules – Conditions/logic that determine how this unit can combine/split with adjacent units.
*   `selection_behavior`: Enum {Expand, Contract, Reveal, TriggerAction, Static} - Defines what happens upon user selection.
*   `action_payload`: JSON – Data associated with `TriggerAction` selection behavior. Could contain URLs, API calls, or other instructions.
*   `dependency_list`: List of `unit_id` - Units this BTU relies on for correct functionality.

**2. Composition Rule Engine:**

*   **Rule Types:**
    *   `Proximity`: Combine if adjacent units share semantic similarity (determined by NLP).
    *   `Delimiter`: Combine/Split based on pre-defined characters (e.g., commas, periods).
    *   `Keyword`: Combine/Split based on presence of specific keywords.
    *   `UserDefined`: Allow developers to create custom rules via scripting.
*   **Conflict Resolution:** Implement a priority system for rules to resolve conflicts when multiple rules apply.

**3. Selection Behavior Implementation:**

*   `Expand`: Selection expands to include adjacent units satisfying composition rules.
*   `Contract`: Selection shrinks, removing adjacent units satisfying composition rules.
*   `Reveal`: Selection reveals hidden content within the unit (e.g., footnotes, translations).
*   `TriggerAction`: Executes an action defined in `action_payload` (e.g., open a related document, display a tooltip).
*   `Static`: Standard selection behavior – no dynamic interaction.

**4. Encoding Scheme:**

*   Extend the existing signal system to include behavior tags.
*   `Signal_BTU_Start`: Marks the beginning of a BTU.
*   `Signal_BTU_End`: Marks the end of a BTU.
*   `Signal_Behavior`:  Integer representing the `selection_behavior` enum value.
*   `Signal_RuleSet`: Encoded list of composition rules.
*   `Signal_ActionPayload`: Encoded JSON payload for `TriggerAction`.

**Pseudocode (BTU Creation & Interaction):**

```
function createBTU(text, rules, behavior, payload):
  unit_id = generateUUID()
  btu = {
    text_segment: text,
    unit_id: unit_id,
    composition_rules: rules,
    selection_behavior: behavior,
    action_payload: payload
  }
  encodeBTU(btu) //Encode with the signals
  return btu

function handleSelection(selected_text, encoded_text):
  btu = decodeBTU(encoded_text, selected_text)

  if btu:
    if btu.selection_behavior == Expand:
      expandBTU(btu)
    elif btu.selection_behavior == Contract:
      contractBTU(btu)
    elif btu.selection_behavior == TriggerAction:
      executeAction(btu.action_payload)

function expandBTU(btu):
  // Find adjacent units satisfying btu.composition_rules
  // Extend the selection to include them
  pass

function contractBTU(btu):
  //Find adjacent units satisfying btu.composition_rules
  //Remove them from the selection
  pass

function executeAction(payload):
  //Execute the action specified in the payload
  pass
```

**Potential Applications:**

*   **Interactive Documents:** Enable users to dynamically customize and explore documents.
*   **Adaptive Learning:** Create educational materials that adapt to user interaction.
*   **Smart Contracts:** Define complex agreements with dynamic terms.
*   **Dynamic UI Elements:** Create user interfaces that respond to user actions in real-time.