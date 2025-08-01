# 12108189

## Dynamic Collaborative Storytelling Engine

**Concept:** Extend the in-call activity recommendation system to facilitate a collaborative storytelling experience *during* the video call, dynamically woven into the shared video feed.

**Specifications:**

**I. Core Engine – Narrative Generation & Adaptation**

1.  **Story Seed Input:** The system begins with a database of story seeds – brief prompts (e.g., "A lost robot finds a friend," "The last bookstore on Earth," "A mysterious signal from space"). These seeds are tagged with genre, complexity, and estimated duration.
2.  **Participant Contribution System:**
    *   Each participant has a designated “Contribution Channel” – a text input/voice-to-text interface.
    *   Participants submit “Story Fragments” – short sentences or phrases that continue the story.
    *   A ‘Pause’ button allows participants to ‘claim’ writing control for a limited time. 
3.  **AI Story Weaver:** An AI analyzes incoming Story Fragments based on:
    *   **Narrative Coherence:**  Is the fragment logically connected to the existing story?
    *   **Style Matching:** Does the fragment match the established tone and voice?
    *   **Participant Influence Score:**  A weighting system based on prior contributions (encourage participation, not domination).
4.  **Dynamic Story Assembly:**
    *   The AI selects the most suitable Story Fragment.
    *   It appends it to the growing narrative.
    *   It generates a revised story synopsis for all participants to view.

**II. Visual Integration – Shared Narrative Canvas**

1.  **Augmented Reality Overlays:** The story text is displayed *within* the video call feed as AR overlays – appearing as if ‘written’ on surfaces in the participants' environments. (e.g., text floating near a participant’s head, scrawled on a virtual ‘wall’ behind them).
2.  **Character/Scene Generation:** The AI generates simple 2D/3D character/scene visualizations based on the story’s content, displayed as AR overlays.  Participants can customize these visuals through simple, in-call commands (e.g., “Change robot color to blue”, “Add a forest background”).
3.  **Mood/Atmosphere Control:** Dynamically adjusts lighting and color palettes of the AR overlays based on the story’s mood (e.g., dark and stormy for suspense, bright and cheerful for happiness).

**III. System Architecture**

1.  **Client-Side Modules:**
    *   Video/Audio Capture & Streaming.
    *   AR Rendering Engine.
    *   Contribution Channel Interface.
    *   Story Display & Control Panel.
2.  **Server-Side Modules:**
    *   Communication Server (Handles video/audio streams).
    *   AI Story Weaver (Narrative Analysis, Generation, Selection).
    *   Story Database (Seed prompts, genre tags).
    *   User Profile Management (Stores user preferences, contribution history).

**IV. Pseudocode – Core Logic (AI Story Weaver)**

```
FUNCTION select_fragment(fragments, current_story):
  scores = {}
  FOR fragment IN fragments:
    coherence_score = calculate_coherence(fragment, current_story)
    style_score = calculate_style_match(fragment, current_story)
    influence_score = get_participant_influence(fragment.author)
    total_score = (coherence_score * 0.5) + (style_score * 0.3) + (influence_score * 0.2)
    scores[fragment] = total_score

  best_fragment = max(scores, key=scores.get)
  return best_fragment
```

**V. Potential Extensions:**

*   **Genre/Theme Selection:** Allow participants to collectively choose a genre or theme before starting.
*   **Character Roles:** Assign roles to participants (e.g., narrator, protagonist, antagonist).
*   **Voice Synthesis:** Have the AI read out the evolving story in different voices.
*   **Automated Illustration:** Generate more detailed illustrations based on the story content.
*   **Integration with External Content:** Incorporate images, music, or video clips into the narrative.