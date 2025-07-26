# 11166051

## Personalized Event 'Remixing' for Dynamic Content Streams

**Concept:** Expand beyond selecting content *based* on detected events and subscription profiles to allow users to actively *remix* event content streams in real-time, creating personalized viewpoints. This goes beyond simply choosing *what* to see, to actively shaping *how* it's presented.

**Specs:**

**1. Real-Time User Interface (UI) – ‘Event Composer’:**

*   **Multi-Layered View:** A UI displaying multiple synchronized video/audio feeds sourced from the input sources (playing field, commentators, audience, etc. - as defined in the base patent).  These are presented as distinct ‘tracks’ within the Event Composer.
*   **Track Visibility/Opacity Control:** Each track has individual toggles for visibility and opacity adjustment. Users can blend feeds to create custom perspectives (e.g., 70% playing field view, 30% commentator audio).
*   **'Focus' Mode:**  Allows users to isolate a single input source and enlarge it to full-screen while maintaining audio from other sources.  Temporary ‘snapshots’ of this view are timestamped and storable for later replay/sharing.
*   **Annotation Layer:** Users can add real-time text annotations and simple drawings (highlights, arrows) directly onto the combined video feed.  These annotations are persistent for the user's session and optionally shareable.
*   **Audio Mixing Console:**  A basic audio mixer to adjust volume levels and apply simple effects (EQ, noise reduction) to individual audio tracks.

**2. Server-Side Event Graph & Remap Logic:**

*   **Event Graph:** The video packaging and origination service maintains a dynamic graph representing detected events and their relationships to specific segments of the input feeds. This extends the existing event detection - beyond simply *knowing* an event occurred, but *where* within each feed it is represented.
*   **Remap Requests:**  The Event Composer UI sends ‘Remap Requests’ to the server, specifying desired track visibility, opacity, audio levels, and any user-added annotations.
*   **Dynamic Stream Composition:** The server receives the Remap Request and, using the Event Graph, dynamically re-composes the content stream. This involves selecting the appropriate segments from each input feed, applying requested visual/audio effects, and integrating user-added annotations.  This stream is then delivered to the user.
*   **Contextual Data Integration:** Allow external data sources (statistics, social media feeds) to be integrated into the Event Composer UI and potentially the dynamically generated stream, based on the detected event.

**3. Pseudocode for Dynamic Stream Composition:**

```
function composeStream(remapRequest, eventGraph, inputFeeds):
  outputStream = new Stream()
  for segment in eventGraph.currentEvent.segments:
    compositeSegment = new Segment()
    for track in remapRequest.tracks:
      if track.visible:
        sourceSegment = inputFeeds[track.source].getSegment(segment.timestamp)
        compositeSegment.addTrack(sourceSegment.applyEffects(track.effects))
    outputStream.addSegment(compositeSegment)
  return outputStream
```

**4.  Subscription Model Extension:**

*   **'Remix' Profiles:** Introduce 'Remix' profiles within the subscription model. These profiles pre-configure common viewing preferences (e.g., ‘Coach’s View’ – focuses on tactical field view, ‘Fanatic’s View’ – emphasizes crowd reactions and commentator audio).
*   **Remix Sharing & Marketplace:** Allow users to save and share their custom ‘Remix’ configurations with other users.  A potential future development could be a marketplace for buying/selling popular Remix profiles.