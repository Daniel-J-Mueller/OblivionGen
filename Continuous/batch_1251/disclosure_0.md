# 10065122

## Dynamic Difficulty Adjustment via Spectator-Driven AI Persona Selection

**Concept:** Extend spectator influence beyond direct event modification to encompass the *style* of AI controlling opposing forces or non-player characters (NPCs) within the game. Spectators don't just change *what* happens, but *how* it happens.

**Specs:**

*   **AI Persona Library:** Develop a diverse library of pre-defined AI "personas," each representing a distinct playstyle (e.g., "Aggressive Rusher," "Defensive Turtle," "Tactical Planner," "Erratic Trickster," "Supportive Teammate"). Each persona is a configuration of several factors:
    *   **Aggression Level:** (0-100) Dictates how often the AI initiates conflict or pursues objectives.
    *   **Risk Tolerance:** (0-100) Influences the AI's willingness to take chances or attempt difficult maneuvers.
    *   **Resource Management Style:** (Conservative, Balanced, Aggressive) Affects how the AI utilizes in-game resources.
    *   **Tactical Preference:** (Close Combat, Long Range, Support, Flanking) Determines the AI's favored combat tactics.
    *   **Personality Traits:** (e.g. "Predictable", "Unpredictable", "Opportunistic", "Patient") influencing the AI's broader behavioral patterns.
*   **Spectator Voting System:**
    *   Each spectator receives a limited pool of "Influence Points" per game session.
    *   Spectators can spend Influence Points to "vote" for the desired AI persona to be applied to a specific opposing force or NPC.
    *   Voting is weighted: More Influence Points spent on a persona increases its probability of being selected.
    *   A "Majority Rules" system determines the final persona, but with a degree of randomness to prevent a single spectator from completely controlling the AI.
*   **Dynamic Persona Switching:**
    *   Allow spectators to "re-vote" on AI personas at predefined intervals or based on specific in-game events (e.g., after a major victory or defeat).
    *   Implement a "Cooldown" period to prevent rapid switching of personas.
*   **Persona Blend/Hybridization:**
    *   Allow spectators to blend aspects of multiple personas together, creating a hybrid AI with unique characteristics. For example, blending "Aggressive Rusher" with "Tactical Planner" might create an AI that initiates frequent attacks but also employs strategic positioning and flanking maneuvers.
*   **AI Persona Visual Feedback:**
    *   Display a visual representation of the currently active AI persona to both players and spectators. This could be a character portrait, a descriptive text label, or a dynamic animation.
*   **Influence Point Earning System:**
    *   Award Influence Points to spectators based on their engagement with the game (e.g., watching key moments, making accurate predictions, participating in in-game events).

**Pseudocode:**

```
// Game Loop

function updateAI(opponentForce) {
  // Calculate spectator votes for each AI persona
  personaVotes = calculatePersonaVotes(opponentForce);

  // Determine the winning AI persona based on votes and randomness
  winningPersona = selectPersona(personaVotes);

  // Apply the winning persona's configuration to the opponentForce's AI
  applyPersonaConfig(opponentForce, winningPersona);
}

function calculatePersonaVotes(opponentForce) {
  // Collect votes from all spectators for available personas
  votes = collectSpectatorVotes(opponentForce);

  // Total votes for each persona
  personaVotes = aggregateVotes(votes);

  return personaVotes;
}

function selectPersona(personaVotes) {
  // Calculate total votes
  totalVotes = sum(personaVotes);

  // Calculate probability for each persona
  personaProbabilities = calculateProbabilities(personaVotes, totalVotes);

  // Select a persona based on probabilities
  selectedPersona = weightedRandomChoice(personaProbabilities);

  return selectedPersona;
}

function applyPersonaConfig(opponentForce, persona) {
  opponentForce.aggressionLevel = persona.aggressionLevel;
  opponentForce.riskTolerance = persona.riskTolerance;
  opponentForce.resourceManagementStyle = persona.resourceManagementStyle;
  opponentForce.tacticalPreference = persona.tacticalPreference;
  opponentForce.personalityTraits = persona.personalityTraits;
}
```