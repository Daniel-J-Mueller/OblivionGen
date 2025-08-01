# 11216152

## Dynamic Personal Space Sculpting

**Concept:** Extend the personal space concept beyond a simple container for content. Allow users to *sculpt* their personal space within the shared 3D environment, altering its physical properties (size, shape, texture, opacity) and even implementing simple physics interactions.

**Specifications:**

**1. Core System – Personal Space Definition:**

*   **Data Structure:**  `PersonalSpace { userID: string, centerPoint: Vector3, radius: float, shapePreset: enum (sphere, cube, custom), textureID: string, opacity: float, physicsEnabled: bool, collisionRadiusMultiplier: float }`
*   **Initialisation:** When a user initiates their personal space, the system creates a `PersonalSpace` object associated with their `userID` and positions it relative to their virtual avatar's head position (initial `centerPoint`). Default `radius` = 1.5m, `shapePreset` = sphere, `opacity` = 0.7, `physicsEnabled` = false, `collisionRadiusMultiplier` = 1.0
*   **Rendering:** The system dynamically renders a visual representation of the `PersonalSpace` based on its properties. Uses a shader allowing for variable opacity and texture application.

**2. Sculpting Interface:**

*   **Input Modalities:** Support sculpting via VR hand tracking, voice commands, and traditional controller input.
*   **Sculpting Tools:**
    *   **Resize:** Allows modification of the `radius` of the `PersonalSpace`.
    *   **Reshape:** Allows modification of the `PersonalSpace` shape from preset options (sphere, cube) or allows drawing a custom shape with a point cloud.
    *   **Texture/Material Selection:** Provides a library of materials and textures that can be applied to the `PersonalSpace`.
    *   **Opacity Control:** A slider control to adjust the `opacity` of the `PersonalSpace`.
    *   **Physics Toggle:** Enables/disables physics interactions for the `PersonalSpace`.
*   **Real-Time Updates:** All sculpting actions are reflected in real-time for the user and visible to other users (subject to network latency).

**3. Physics Integration (Optional):**

*   If `physicsEnabled` is true, the `PersonalSpace` acts as a collision volume. Content items within the `PersonalSpace` can be affected by simulated gravity and collisions with other objects.
*   The `collisionRadiusMultiplier` parameter allows scaling the effective collision radius of the `PersonalSpace` for fine-tuned physics interactions.
*   Basic “push/pull” interactions – Users can physically ‘push’ content items around within their personal space.

**4. Network Synchronization:**

*   `PersonalSpace` data (centerPoint, radius, shapePreset, textureID, opacity, physicsEnabled, collisionRadiusMultiplier) is synchronised between all users.
*   Optimisation: Data is synchronised only when properties change significantly.
*   Compression: Implement data compression to reduce bandwidth usage.

**5. Pseudocode – Sculpting Action:**

```
function ApplySculptingAction(userID, actionType, parameters) {
  personalSpace = GetPersonalSpace(userID)

  switch (actionType) {
    case "Resize":
      personalSpace.radius = parameters.newRadius
      break
    case "Reshape":
      personalSpace.shapePreset = parameters.newShape
      break
    case "Texture":
      personalSpace.textureID = parameters.newTextureID
      break
    case "Opacity":
      personalSpace.opacity = parameters.newOpacity
      break
    case "PhysicsToggle":
      personalSpace.physicsEnabled = parameters.newPhysicsEnabled
      break
  }

  SynchronizePersonalSpaceData(userID, personalSpace)
  RenderPersonalSpace(personalSpace)
}
```

**Potential Applications:**

*   Enhanced privacy: Users can create a more secluded space.
*   Creative expression: Users can customise their personal space to reflect their personality.
*   Collaborative workspaces: Users can create shared personal spaces for collaborative tasks.
*   Gamification: Personal space could be integrated into game mechanics.