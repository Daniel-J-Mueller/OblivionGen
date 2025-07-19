# 10244203

## Dynamic Caption Style Injection

**Concept:** Extend the existing caption channel re-packing system to allow for dynamic injection of visual styles (fonts, colors, backgrounds, positioning) *alongside* the caption text itself. This goes beyond simply selecting a language; it allows for branding, accessibility overrides, and immersive experiences.

**Specs:**

**1. Style Metadata Channel:**

*   **Addition:** Introduce a new metadata channel alongside the existing caption channels and subtitle placeholder. This “Style Channel” will contain instructions for rendering the captions.
*   **Format:** Style Channel data will be encoded as a compact binary format (e.g., a custom JSON-like structure) containing style directives.  Examples:
    *   `fontFamily: "Arial"`
    *   `textColor: "#FFFFFF"`
    *   `backgroundColor: "#000000"` (with transparency options)
    *   `strokeColor: "#FF0000"` (outline color)
    *   `strokeWidth: 2`
    *   `shadowColor: "#000000"`
    *   `shadowOffset: [2, 2]`
    *   `position: "bottom"` / "top" / "center"
    *   `alignment: "left" / "right" / "center"`
*   **Synchronization:** Each Style Channel entry is time-stamped to align with specific caption entries.

**2. Re-Packager Modification:**

*   **Style Injection:** The re-packager, upon selecting a caption channel, will *also* read the corresponding Style Channel entries.
*   **Style Merging:**  The re-packager will merge the selected style directives with a base style profile (defined system-wide or by the content provider). This allows for a consistent look while enabling overrides.
*   **Style Channel Priority:**  Implement a priority system for style directives. For example, user-defined accessibility overrides would take precedence over content provider styles.

**3. Player Modification:**

*   **Style Parsing:** The player will need to parse the injected style directives.
*   **Rendering Engine Integration:** The player’s rendering engine must be able to interpret and apply the style directives to the displayed captions.  The rendering engine should ideally support a pluggable style sheet architecture to support different rendering techniques.
*   **Dynamic Updates:** The player should be able to dynamically update the caption styles mid-stream based on the injected directives.

**Pseudocode (Re-Packager):**

```
function rePackageStream(bitstream, selectedCaptionChannel):
  video = bitstream.getVideo()
  styleChannel = bitstream.getStyleChannel(selectedCaptionChannel)
  captionChannel = bitstream.getCaptionChannel(selectedCaptionChannel)
  baseStyle = getBaseStyleProfile() // System or content-defined
  mergedStyle = mergeStyles(baseStyle, styleChannel)

  rePackagedBitstream = new Bitstream(video, mergedStyle, captionChannel)
  return rePackagedBitstream
end function

function mergeStyles(baseStyle, styleChannel):
  merged = copy(baseStyle)
  for directive in styleChannel:
    merged[directive.key] = directive.value
  return merged
end function
```

**Potential Applications:**

*   **Branded Captions:**  Allow content providers to visually brand their captions.
*   **Accessibility:**  Enable users to customize caption appearance for optimal readability (e.g., high contrast, large fonts).
*   **Immersive Experiences:**  Synchronize caption styles with on-screen events (e.g., a character speaking in red captions).
*   **Multi-Language Styling:** Apply different visual styles to captions in different languages for easy identification.