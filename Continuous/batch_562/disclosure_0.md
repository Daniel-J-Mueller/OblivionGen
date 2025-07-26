# 9870391

**Dynamic Proximity Scoring with Skill/Project Tagging & AI-Driven 'Serendipity' Boost**

**Concept:** Extend the proximity ranking beyond location and department to include skill sets, current project involvement, and introduce an AI-driven 'serendipity' factor to surface potentially valuable connections users *wouldn't* normally find.

**Specs:**

1.  **Enhanced Profile Data:**
    *   Add fields for: `Skills (list)`, `Current Projects (list)`, `Desired Project Involvement (list)`.  These should be taggable/selectable from a predefined taxonomy (or allow for free-text entry with NLP tagging).
    *   Profile 'weighting' sliders – Users can indicate importance of each data point (location, department, skills, projects) in their search.

2.  **Proximity Score Algorithm – Modified:**
    *   `Proximity Score = (LocationWeight * LocationProximity) + (DepartmentWeight * DepartmentProximity) + (SkillWeight * SkillMatchScore) + (ProjectWeight * ProjectOverlapScore) + SerendipityBoost`
        *   `LocationProximity`: Distance calculation as per existing patent, potentially with floor/building specificity.
        *   `DepartmentProximity`:  Binary (same/different) or hierarchical distance (e.g., Finance & Accounting are closer than Finance & Engineering).
        *   `SkillMatchScore`:  Calculated based on overlap between user’s desired skills and profile’s skills. Use a scoring function (e.g., Jaccard index, cosine similarity).
        *   `ProjectOverlapScore`: Binary (shared project) or weighted (number of shared projects, project importance).
        *   `SerendipityBoost`: AI-driven component (see below).

3.  **AI 'Serendipity' Engine:**
    *   **Data Source:** Anonymized usage data – search terms, profile views, connection requests, project collaboration patterns.
    *   **Model:**  A recommendation engine (collaborative filtering, content-based filtering, or hybrid).
    *   **Function:** The AI analyzes user profiles and search queries to identify *latent* connections – people with complementary skills, individuals who have collaborated successfully on unrelated projects, those who might benefit from knowledge sharing even if there’s no direct overlap.
    *   **Output:** A 'SerendipityBoost' value (0-1).  Higher values increase the profile’s ranking.  This value should be *explainable* – provide a short justification (e.g., "This person has experience with technology X which complements your current project.").
    *   **Dynamic Learning:** The AI continuously learns from user interactions – if a user ignores a 'serendipitous' suggestion, the AI adjusts its algorithm accordingly.

4.  **UI/UX Considerations:**
    *   **Advanced Search Filters:** Allow users to specify skill requirements, project interests, and desired level of 'serendipity'.
    *   **Profile 'Compatibility' Score:** Display a visual indicator of how well a profile matches the user’s search criteria and 'serendipity' preferences.
    *   **'Explore' Mode:** A dedicated mode that focuses on surfacing unexpected connections based on the AI’s recommendations.
    *   **Explainable AI:** Display the rationale behind the 'SerendipityBoost' value for each profile.

**Pseudocode:**

```
function calculateProximityScore(userProfile, targetProfile, userWeights):
  locationProximity = calculateLocationProximity(userProfile.location, targetProfile.location)
  departmentProximity = calculateDepartmentProximity(userProfile.department, targetProfile.department)
  skillMatchScore = calculateSkillMatchScore(userProfile.skills, targetProfile.skills)
  projectOverlapScore = calculateProjectOverlapScore(userProfile.projects, targetProfile.projects)
  serendipityBoost = getSerendipityBoost(userProfile, targetProfile)

  proximityScore = (userWeights.location * locationProximity) +
                   (userWeights.department * departmentProximity) +
                   (userWeights.skill * skillMatchScore) +
                   (userWeights.project * projectOverlapScore) +
                   serendipityBoost

  return proximityScore

function getSerendipityBoost(userProfile, targetProfile):
  // AI-driven recommendation engine logic here
  // Returns a value between 0 and 1
  // Based on latent connections, complementary skills, etc.
  return aiRecommendationEngine.getRecommendationScore(userProfile, targetProfile)
```