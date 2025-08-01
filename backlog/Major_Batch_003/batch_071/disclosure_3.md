# 11570590

## Dynamic Group Focus - "Ephemeral Context"

**Specification:** A system extending group messaging beyond pinned messages, enabling temporary, shared contextual 'layers' over ongoing conversations.

**Concept:** The existing pinned message functionality is good for persistent reminders, but limits real-time collaboration around specific topics *within* a group chat.  Ephemeral Context allows users to temporarily highlight sections of the chat *and* attach related data, accessible to all group members *until the context expires*.  

**Technical Specs:**

1.  **Context Creation:**  A user selects a contiguous block of messages (a 'span') within the chat.  This span becomes the base for the ephemeral context.
2.  **Data Association:** The user can then attach one or more of the following data types to this context:
    *   **Quick Poll:** A simple multiple-choice poll relating to the selected messages. Results visible to all.
    *   **Shared Sketchpad:** A collaborative, real-time sketchpad overlaid on the chat, accessible by all group members.
    *   **Linked Document:** A link to a document (Google Doc, PDF, etc.) relevant to the selected messages. Preview within the chat, full access via link.
    *   **Task List:** A dynamically updating task list, assignable to group members.
    *   **Location Pin:** A map pin related to the discussion in the chat span.
3.  **Context Display:** The selected message span is visually highlighted (e.g., background color change). An indicator icon appears, signifying an active ephemeral context.  A collapsed panel displays the associated data.
4.  **Context Expiration:** Each context has a configurable expiration timer (e.g., 5 minutes, 15 minutes, 30 minutes, 1 hour, until manually dismissed).  A visual countdown is displayed.
5.  **Context Management:**
    *   A dedicated ‘Contexts’ tab lists all active and recently expired contexts for the group.
    *   Users can extend the expiration timer (subject to group admin permissions).
    *   Group admins can globally dismiss contexts.

**Pseudocode (Context Creation):**

```
function createContext(messageSpan, dataType, dataPayload, expirationTime) {
  contextID = generateUniqueID();
  contextData = {
    id: contextID,
    span: messageSpan,
    dataType: dataType,
    data: dataPayload,
    expirationTime: expirationTime,
    creator: userID
  };

  // Store contextData in database/cache

  // Update chat UI: Highlight messageSpan, add context indicator

  // Send notification to group members
}
```

**Novelty:**  Moves beyond static pinning to *dynamic*, time-bound layers of contextual information. Transforms group chat from solely a communication channel into a collaborative workspace.  Addresses the issue of 'context loss' in long, rapidly moving chats.  Not simply adding features; it's altering the *fundamental interaction model* of the group chat.