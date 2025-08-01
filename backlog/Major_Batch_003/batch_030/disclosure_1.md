# 11379103

## Dynamic Contextual Content "Portals"

**Concept:** Extend the contextual content sharing beyond static images/stickers to interactive, short-duration "portals" embedded within the embellished content. These portals allow a limited, real-time interaction between users – akin to a fleeting shared AR space.

**Specs:**

*   **Portal Creation:** When a user creates a contextual content item (the "portal host"), they define a short duration (e.g., 5-30 seconds) for the portal's active state. They also select a limited interaction type:
    *   **"Reaction Stream":** Viewers can send quick, animated reactions (emojis, pre-defined GIFs) that appear *within* the portal on the host's view for the duration.
    *   **"Quick Sketch":** Viewers can contribute a single, ephemeral line/shape to a shared canvas within the portal.
    *   **"Sound Bite":** Viewers can record & send a very short audio clip (1-3 seconds) which plays on the host’s side.
*   **Portal Activation:**  When a user views embellished content containing a portal, a visual indicator (e.g., pulsing aura, subtle animation) suggests interaction.  Tapping/holding the indicator activates the portal.
*   **Real-time Sync (Limited):** A lightweight, peer-to-peer (or via a minimal server relay) connection is established between the portal host & active viewers. This facilitates the real-time syncing of reactions, sketches, or sound bites.  Low latency is prioritized over complex interactions.
*   **Ephemeral Content:** All interactions *within* the portal are strictly ephemeral. They are not saved, archived, or made persistent. The portal automatically closes after the defined duration or if the host disconnects.
*   **Content Feed Integration:** Portals appear as interactive elements within the standard content feed.  A visual cue indicates when a portal is "live" and accepting interactions.
*   **Permission Control:**  The portal host retains full control over who can interact with their portal (using the existing permissioning system).

**Pseudocode (Portal Activation & Interaction):**

```
//On Viewer Side

function onViewEmbellishedContent(content) {
  if (content.hasPortal && content.portalIsActive) {
    displayPortalIndicator(content)

    onPortalIndicatorTap() {
      establishPortalConnection(content.hostUserID)
      requestPortalAccess(content.hostUserID)

      onPortalAccessGranted() {
        startReceivingPortalInteractions()
      }
    }
  }
}

function startReceivingPortalInteractions() {
  //Listen for real-time interactions (reactions, sketches, sound)
  //Display received interactions within the portal UI
}
```

```
//On Host Side

function onPortalRequest(requestingUserID, contentID) {
  if (isUserPermitted(requestingUserID)) {
    grantPortalAccess(requestingUserID)
    startReceivingInteractions(requestingUserID)
  }
}

function startReceivingInteractions(userID) {
  //Listen for real-time interactions from userID
  //Render received interactions on Host's display within the portal UI
}
```

**Hardware Considerations:** Minimal. Relies heavily on existing camera/display capabilities. Requires robust network connectivity for real-time sync.

**Potential Extensions:** Integration with spatial audio (so interactions "sound" like they're coming from within the portal).  A "portal gallery" for browsing recently active portals (subject to privacy controls).