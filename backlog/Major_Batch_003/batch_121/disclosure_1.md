# 11874886

## Dynamic Composition Templates

**Specification:** A system for pre-defined, dynamic composition templates that react to user input *before* content is created, influencing the layout and available metadata options.

**Core Concept:** Instead of presenting a static “composition landing screen” with fixed input spaces, offer a library of templates – think “story starters” – that dynamically restructure the interface based on the selected template. These templates aren't merely visual; they dictate *which* input spaces are presented and their priority.

**Template Examples:**

*   **“Quick Poll”:** Interface focuses on a media space (image/video) and *two* text spaces configured specifically for poll options. Metadata space limited to pre-defined “Poll Duration” selector. Posting space immediately presents “Post to Story” or “Post to Feed with Poll” options.
*   **“Location Check-In + Feeling”:**  Interface emphasizes location selection (integration with map services).  Metadata space prioritized for “Feeling” selection (emoji picker/text input). Text space minimized, for a short caption. Posting defaults to "Share Location + Feeling to Story."
*   **“Recipe Share”:** Media space prioritized for food image/video. Text space structured for ingredient list and instructions (multi-line editor). Metadata space includes “Prep Time,” “Cook Time,” and “Difficulty” selectors.
*   **“Event Announcement”:** Media space for event image/video.  Text space for event details (date, time, location, description).  Metadata space includes event category selector.  Posting space linked to event scheduling services.

**System Components:**

1.  **Template Library:** A database of pre-defined composition templates, each containing:
    *   Layout definition (arrangement and size of input spaces)
    *   Input space types (media, text, metadata, posting)
    *   Metadata option sets (specific metadata available for that template)
    *   Default posting options
2.  **Template Selection Interface:**  A visually accessible library of templates presented to the user upon initiating composition. Templates categorized by use case.
3.  **Dynamic Interface Builder:**  A module that renders the composition landing screen based on the selected template. This module dynamically creates and arranges input spaces, populates metadata options, and configures posting options.
4.  **Input Validation:** Enforces template-specific input constraints (e.g., maximum character count for poll options, required fields for event announcements).
5.  **Content Integration:**  Connects input spaces to content sources (camera, gallery, map services, scheduling apps).

**Pseudocode (Dynamic Interface Builder):**

```
function buildCompositionScreen(templateID) {
  template = getTemplateFromLibrary(templateID);

  screen = new CompositionScreen();

  for (inputSpace in template.inputSpaces) {
    spaceType = inputSpace.type;
    spaceConfig = inputSpace.config;

    if (spaceType == "media") {
      mediaSpace = new MediaSpace(spaceConfig);
      screen.addSpace(mediaSpace);
    } else if (spaceType == "text") {
      textSpace = new TextSpace(spaceConfig);
      screen.addSpace(textSpace);
    } else if (spaceType == "metadata") {
      metadataSpace = new MetadataSpace(spaceConfig);
      screen.addSpace(metadataSpace);
    }
  }

  postingSpace = new PostingSpace(template.defaultPostingOptions);
  screen.addSpace(postingSpace);

  return screen;
}
```

**Further Considerations:**

*   **User-Created Templates:** Allow users to create and share their own templates.
*   **AI-Powered Template Recommendations:**  Recommend templates based on user activity and context.
*   **Template Customization:** Allow users to customize existing templates (e.g., change color schemes, font sizes).
*   **Animation and Transitions:** Incorporate animations and transitions to enhance the user experience.