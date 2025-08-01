# 9299064

## Dynamic Skill-Based Matchmaking & Tiered Reward System

**Concept:** Expand beyond simple tier placement on a scoreboard to actively utilize tiered rankings for dynamic, skill-based matchmaking *within* the game itself, coupled with a reward system that adapts based on both skill *and* social connections.

**Specs:**

*   **Skill Metric:** Implement a continuously updating Elo-like rating system for each player. This rating isn't directly visible, but drives matchmaking and tier assignment.
*   **Tier Assignment:** Players are assigned to tiers based on percentile ranges of their Elo rating, as the patent already outlines. However, tier boundaries dynamically shift based on overall player population distribution. (e.g., if a large influx of new players enters, the lower tiers expand).
*   **Matchmaking Protocol:**
    *   The system prioritizes matching players within a narrow Elo rating range (e.g., +/- 100 points) and *same tier*.
    *   If a suitable match isn't found within the same tier, the system *expands* the search to adjacent tiers, weighted by Elo difference. (Slight Elo difference within adjacent tiers is acceptable to reduce queue times).
    *   **Social Connection Bonus:** If a player has a confirmed social connection (friend list, common contacts) with another player within the acceptable Elo/Tier range, this pairing is *prioritized heavily*. The system should actively suggest matches with social connections.
*   **Dynamic Reward System:**
    *   **Base XP/Rewards:** XP and in-game currency are awarded based on match outcome and player performance (kills, assists, objectives completed).
    *   **Tier Multiplier:** Rewards are multiplied based on the player’s tier. Higher tiers receive significantly higher multipliers.
    *   **Social Connection Bonus:** An *additional* reward multiplier is applied if the player is matched with, and plays *with*, a social connection. The multiplier increases with the strength of the social connection (e.g., shared close friends get a bigger bonus than just a casual contact).
    *   **"Streak" System:** Maintain a "social streak" counter. Consecutive matches played *with* social connections award increasing bonuses. Losing the streak (playing solo or with randoms) resets the counter.
*   **Client-Side UI:**
    *   Display a “Social Matchmaking” toggle. Enabling this prioritizes matches with social connections, potentially increasing queue times.
    *   Show a “Social Streak” counter prominently in the UI.
    *   Display a small icon indicating when a match includes a social connection.
    *   Subtle highlighting of social connection player names in match lobbies.
*   **Backend Logic:**
    *   Asynchronous job queue to update Elo ratings after each match.
    *   Caching layer for Elo ratings to minimize database load.
    *   Real-time event bus to notify clients of Social Streak updates.
*   **Pseudocode (Matchmaking):**

```
function findMatch(player):
    tier = getPlayerTier(player)
    elo = getPlayerElo(player)
    potentialMatches = findPlayersInTier(tier)
    
    # Filter for Elo range
    potentialMatches = filter(potentialMatches, function(p) { return abs(getPlayerElo(p) - elo) < 150 })

    #Prioritize social connections
    socialMatches = filter(potentialMatches, function(p) { isSocialConnection(player, p) })
    if(socialMatches.length > 0):
      return random(socialMatches)

    # If no social matches, return a random match from the remaining potential matches
    if(potentialMatches.length > 0):
      return random(potentialMatches)

    #Expand search to adjacent tiers if no matches found
    adjacentTiers = getAdjacentTiers(tier)
    for adjacentTier in adjacentTiers:
      potentialMatches = findPlayersInTier(adjacentTier)
      #Elo filtering as before
      if(potentialMatches.length > 0):
        return random(potentialMatches)
    
    #If still no matches, increase search range
    return null //No match found
```

This expands on the patent’s scoreboard concept by actively integrating tier and social connections into the gameplay experience. It promotes player engagement, encourages social interaction, and creates a more rewarding and competitive environment.