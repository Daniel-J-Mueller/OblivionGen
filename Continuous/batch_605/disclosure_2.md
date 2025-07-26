# 9021526

## Adaptive Resolution Preview Tiles

**Concept:** Extend the progressive granularity preview concept by dynamically adjusting the resolution of preview "tiles" based on network conditions *and* user interaction.

**Specification:**

**1. Tile Generation:**

*   Divide the video into a grid of rectangular regions (tiles). Tile size is configurable, but a starting point is 64x36 pixels.
*   Generate multiple versions of *each* tile at varying resolutions: Low (16x9), Medium (32x18), High (64x36), and potentially Very High (128x72).
*   These multiple resolutions are stored as separate, uniquely identified image files (e.g., tile_01_low.jpg, tile_01_medium.jpg, etc.). A metadata file maps tile ID to available resolutions.

**2. Initial Preview Delivery:**

*   Deliver the lowest resolution tiles initially. Arrange these in the same grid pattern as the original video tiling. This provides a *very* low-granularity, rapidly-loading preview. The level order traversal method outlined in the reference patent can be used for tile selection during initial delivery.

**3. Adaptive Resolution Switching:**

*   **Network Monitoring:** Continuously monitor the client's network bandwidth.
*   **User Interaction Monitoring:** Track user scrolling/seeking behavior within the preview. If the user is actively scrubbing or zooming, prioritize higher-resolution tile delivery for the relevant area.
*   **Resolution Switch Algorithm:**

    ```pseudocode
    function determine_tile_resolution(network_bandwidth, user_interaction_level, current_resolution):
      if network_bandwidth > HIGH_THRESHOLD:
        if user_interaction_level > MEDIUM_THRESHOLD:
          return VERY_HIGH
        else:
          return HIGH
      elif network_bandwidth > MEDIUM_THRESHOLD:
        if user_interaction_level > LOW_THRESHOLD:
          return MEDIUM
        else:
          return LOW
      else:
        return LOW
    ```

    The thresholds (HIGH_THRESHOLD, MEDIUM_THRESHOLD, LOW_THRESHOLD) are configurable.

**4. Tile Request & Delivery:**

*   The client requests tiles based on its viewport and the determined resolution.
*   The server streams the requested tiles on demand.

**5.  Metadata & Indexing:**

*   A master index file maps each tile’s coordinates (video time offset & grid position) to the available resolutions and their corresponding file paths.
*   Metadata associated with each tile includes: timestamp of capture, keyframe flag (indicating potential for higher fidelity), and content-aware characteristics (e.g., amount of motion, dominant colors). This metadata could be used to further optimize tile selection and prioritization.



**Implementation Notes:**

*   This system is designed to be scalable. Tile generation can be pre-processed and cached.
*   The server can use a content delivery network (CDN) to distribute tiles efficiently.
*   The client application needs to be able to dynamically assemble the preview from the received tiles.
*   Error handling mechanisms should be implemented to handle cases where a requested tile is not available.