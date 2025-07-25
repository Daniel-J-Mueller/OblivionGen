# 11074907

## Dynamic Prompt Weighting for Conversational AI

**Specification:** A system for dynamically weighting prompts based on user emotional state, inferred through real-time audio analysis and NLP of user utterances.

**Core Concept:** The patent focuses on detecting repetitive prompts. This builds on that by *actively* adjusting the prompts themselves, not just flagging repetition. If a user is displaying frustration (detected via audio cues – pitch, volume, speed – and NLP – sentiment analysis, keyword detection – like “confused”, “angry”, “doesn’t make sense”), the system shifts *away* from default, scripted prompts and towards more open-ended, exploratory phrasing. Conversely, if the user is calm and receptive, it reinforces the efficiency of the existing prompt structure.

**Components:**

1.  **Emotional State Analyzer:**
    *   **Audio Input:** Real-time audio stream from the user.
    *   **NLP Input:**  Text transcript of user input.
    *   **Analysis Modules:**
        *   *Pitch/Volume/Speed Analysis:* Detects vocal stress markers.
        *   *Sentiment Analysis:* Classifies emotional tone (positive, negative, neutral).
        *   *Keyword Detection:* Identifies frustration/confusion indicators.
    *   **Output:**  Emotional State Score (0-100, with 0 being calm/positive and 100 being frustrated/negative).

2.  **Prompt Database:**
    *   A hierarchical database of prompts, categorized by:
        *   *Intent:* What the prompt is trying to achieve.
        *   *Complexity:*  How much information the prompt delivers.
        *   *Openness:*  How much free-form response the prompt allows. (Scale of 1-10, 1 being highly scripted, 10 being completely open-ended).
    *   Each prompt has multiple variations at different levels of openness.

3.  **Dynamic Prompt Selector:**
    *   **Input:** Emotional State Score, Current Intent.
    *   **Logic:**
        *   If Emotional State Score > 70: Select the *most open* prompt variation for the current intent.
        *   If Emotional State Score > 30 and < 70: Select a *mid-range* prompt variation.
        *   If Emotional State Score < 30: Select the *most scripted* prompt variation.
    *   **Output:**  Selected Prompt.

**Pseudocode:**

```
function select_prompt(emotional_state_score, current_intent):
  prompt_variations = get_prompt_variations(current_intent)

  if emotional_state_score > 70:
    selected_prompt = max(prompt_variations, key=lambda x: x.openness) //Get prompt with highest openness value
  elif emotional_state_score > 30 and emotional_state_score < 70:
    //Select mid-range prompt variation (implementation depends on how variations are structured)
    selected_prompt = select_mid_range_prompt(prompt_variations)
  else:
    selected_prompt = min(prompt_variations, key=lambda x: x.openness) //Get prompt with lowest openness value

  return selected_prompt
```

**Implementation Details:**

*   The “mid-range” prompt selection could be a simple average of openness values, or a more complex algorithm based on user history.
*   The Emotional State Analyzer requires training data to accurately classify emotional states.
*   The system should include a fallback mechanism in case the Emotional State Analyzer fails.
*   Consider integrating a “User Preference” profile – if a user consistently prefers open-ended responses, override the emotional state weighting.