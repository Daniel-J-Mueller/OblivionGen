# 11741435

## Dynamic Session "Bubbles" & Contextual Proximity

**Concept:** Expand the scheduling interface beyond direct requests and integrate a visually dynamic, spatially aware "bubble" system representing session availability and proximity to user interests/connections.

**Specifications:**

**1. Core Data Structures:**

*   `SessionBubble`:
    *   `bubbleID`: Unique identifier.
    *   `providerID`: ID of the service provider.
    *   `sessionType`: (String) - Type of service (e.g., "Consultation", "Training", "Demo").
    *   `availability`: (Array of DateTime objects) – Specific times the session is available.
    *   `geoCoordinates`: (Latitude, Longitude) – Location of the session (physical or virtual “location” for online sessions – could be based on provider’s primary location or specific virtual event link).
    *   `interestScore`: (Float 0-1) – Score representing relevance to the user's expressed interests and connection network.
    *   `visualRadius`: (Integer) – Determines the size of the bubble on the map/interface. Directly tied to `interestScore` and session duration.
    *   `sessionDetailsURL`: URL for detailed session information.

**2. Interface/Visualization:**

*   **“Explore” View:**  A map-based (or scrollable list) interface showing `SessionBubbles` around the user. The bubbles visually expand/contract based on `interestScore` and session duration. Higher score = larger bubble.
*   **Bubble Interaction:** Tapping/Clicking a bubble displays a concise session summary (provider name, session type, availability, and “Request” button).
*   **Filtering:** Options to filter bubbles by:
    *   `sessionType`
    *   `geoDistance` (range in miles/km)
    *   `providerNetwork` (show bubbles from connections or connections-of-connections)
    *   `priceRange`

**3. Dynamic Bubble Generation & Adjustment:**

*   **Interest Scoring Algorithm:**
    *   User Interest Profile: Build a profile based on user activity (tags followed, providers connected to, previous sessions booked).
    *   Provider Tagging: Providers tag sessions with relevant keywords/categories.
    *   Interest Matching: Match provider tags with user interests.
    *   Connection Weighting: Increase the `interestScore` if the provider is a connection or a connection-of-connection.
*   **Real-time Availability Update:** Bubbles dynamically adjust size and visibility based on session availability.  If a session is booked, the bubble shrinks and fades, or disappears.
*    **Contextual Bubble Generation:** Beyond user interest, session bubbles appear based on the user's *current* context. For instance, if the user is at a conference venue (detected via location services), bubbles representing relevant workshops or sessions at that venue will appear.
*   **“Serendipity” Factor:** A small percentage of bubbles are generated randomly, offering sessions completely outside the user’s typical interests.  This encourages exploration and discovery.

**4. Backend Processes (Pseudocode):**

```
function generateSessionBubbles(userID):
  userProfile = getUserProfile(userID)
  sessionList = getAllAvailableSessions()

  for session in sessionList:
    interestScore = calculateInterestScore(session, userProfile)
    bubble = createSessionBubble(session, interestScore)
    addBubbleToMap(bubble)

  return mapOfBubbles

function calculateInterestScore(session, userProfile):
  tagsMatch = compareTags(session.tags, userProfile.interests)
  connectionWeight = getConnectionWeight(session.providerID, userProfile.connections)
  score = (tagsMatchWeight * tagsMatch) + (connectionWeight * connectionWeight)
  return score

function getConnectionWeight(providerID, userConnections):
  if providerID in userConnections:
    return 0.8
  elif any(connection.providerID == providerID for connection in userConnections.connectionsOfConnections):
    return 0.4
  else:
    return 0.0
```

**5. API Endpoints:**

*   `/sessions/bubbles?userID={userID}`: Returns a list of `SessionBubble` objects.
*   `/sessions/{sessionID}`: Returns detailed information about a specific session.
*   `/requests/new?sessionID={sessionID}&userID={userID}`: Creates a new session request.

This system moves beyond simple scheduling requests and transforms session discovery into a dynamic, spatially aware, and contextually relevant experience. It prioritizes serendipity and expands the user’s potential for engaging with valuable services and providers.