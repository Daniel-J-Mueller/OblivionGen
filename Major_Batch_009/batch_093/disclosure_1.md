# 8392360

## Dynamic Forum Health Scoring & Proactive Question Migration

**Concept:** Enhance the existing system by introducing a dynamic forum health scoring system and using this score to *proactively* migrate potentially unanswered questions *before* a user even posts them.  This moves beyond reactive question transfer to preventative solution finding.

**Specifications:**

**1. Forum Health Scoring Module:**

*   **Data Points:**
    *   *Response Time (RT):* Average time to receive a first response to a new question. (Weighted 30%)
    *   *Resolution Rate (RR):* Percentage of questions receiving a 'good' answer (as defined by the existing system). (Weighted 40%)
    *   *Active User Count (AUC):* Number of unique users posting/responding in the last 7 days. (Weighted 15%)
    *   *Content Freshness (CF):*  Measure of recent activity – ratio of posts within the last 24 hours to total posts. (Weighted 15%)
*   **Scoring Algorithm:**
    *   Normalize each data point to a scale of 0-1.
    *   Weighted sum of normalized data points.  Resulting score range: 0-1.
    *   Thresholds:
        *   0.0 – 0.3: “Critical” – High probability of unanswered questions.
        *   0.3 – 0.6: “Warning” – Moderate risk of unanswered questions.
        *   0.6 – 1.0: “Healthy” – Low risk of unanswered questions.

**2. Predictive Question Migration Module:**

*   **Input:** New question text *before* it is posted.
*   **Topic Modeling:** Employ NLP techniques (e.g., LDA, BERT) to determine the primary topic(s) of the question.
*   **Forum Comparison:**
    *   Query a database of forums and their associated topic profiles.
    *   Calculate a similarity score between the question's topic profile and each forum's topic profile.
    *   Retrieve the top N forums with the highest similarity scores.
*   **Health Score Integration:**
    *   For each of the top N forums, retrieve its current health score.
    *   Prioritize forums with a 'Healthy' or 'Warning' score over those with a 'Critical' score.
*   **Migration Prompt:**
    *   If the originating forum’s health score falls below a certain threshold (e.g., 0.4), *and* a higher-scoring forum exists with a strong topic similarity, a prompt will be displayed to the user *before* posting: “This question may receive a faster/better response in [Alternative Forum Name].  Would you like to post it there instead?”
    *   This prompt includes a link to directly create a new post in the target forum, pre-populated with the question text.

**3.  Automated Shadow Posting (Optional - Requires User Opt-In):**

*   If a user *opts-in* to this feature, and a migration prompt is triggered, the system can *automatically* create a shadow post in the alternative forum *after* the original post.
*   The shadow post includes a link back to the original post, allowing users to consolidate responses.  The shadow post is clearly marked as a duplicate.

**Pseudocode:**

```
function predict_migration(question_text, originating_forum):
  topic_profile = analyze_topic(question_text)
  candidate_forums = find_similar_forums(topic_profile)
  filtered_forums = filter_by_health(candidate_forums) //Remove Critical forums
  if len(filtered_forums) > 0:
    best_forum = select_best_forum(filtered_forums)
    if originating_forum.health_score < 0.4:
      display_migration_prompt(best_forum)
```

**Data Requirements:**

*   Historical data on forum activity (posts, responses, votes, etc.).
*   Forum topic profiles (maintained through content analysis or manual tagging).
*   User preferences (opt-in status for shadow posting).