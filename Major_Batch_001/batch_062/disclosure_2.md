# 10049163

## Personalized Query Expansion via Dynamic Knowledge Graph Construction

**Concept:** Expand search queries not just through pre-existing data stores (taxonomy, brand names), but by *dynamically* building a temporary knowledge graph derived from the user’s recent search history, browsing activity, and potentially, social media data (with user consent). This graph becomes a temporary “personal taxonomy” supplementing the static compilation data store.

**Specs:**

1.  **User Profile Integration:** Establish a secure system to access (with explicit user permission) recent search logs, browsing history (via browser extension or app integration), and optionally, social media feeds.  Data is anonymized/pseudonymized where appropriate.
2.  **Knowledge Graph Construction:**
    *   **Node Creation:** Extract entities (nouns, named entities) from the user's history. These become nodes in the knowledge graph.
    *   **Edge Creation:**  Establish relationships between nodes based on co-occurrence in search queries, browsing sessions, and/or social media posts.  Edge weight reflects frequency/proximity of co-occurrence. Relationship types: "Searched Together," "Viewed Together," "Mentioned Together."
    *   **Graph Storage:** Use a graph database (Neo4j, Amazon Neptune) for efficient storage and retrieval of the dynamic knowledge graph.  Graph has a defined TTL (Time To Live) – e.g., 24 hours – to ensure it reflects recent user behavior.
3.  **Query Enhancement:**
    *   **Phrase Identification:** (As per the original patent) Identify candidate phrases in the user’s current search query.
    *   **Graph Traversal:** When a phrase is identified, traverse the dynamic knowledge graph starting from the phrase's constituent entities.
    *   **Expansion Candidate Generation:**  Identify nodes connected to the phrase's entities.  These represent potential query expansion terms.
    *   **Scoring & Filtering:**  Score expansion candidates based on:
        *   Edge weight (strength of connection in the graph).
        *   Node degree (number of connections the node has).
        *   Relevance to the original query (semantic similarity).
    *   **Query Reformulation:**  Add high-scoring expansion candidates to the original query using Boolean operators (OR, AND) or phrase matching.  Experiment with different query reformulation strategies.
4.  **Hybrid Search:** Combine the reformulated query with the original query and submit both to the catalog of items.  Use a weighted combination of the results from both queries, potentially using machine learning to optimize the weighting based on user feedback.
5.  **User Interface Integration:**  Provide a UI element allowing users to view the dynamically generated expansion terms and adjust the level of expansion (e.g., "Expand Query," "Expand Query More"). Also, a UI element to temporarily disable dynamic expansion for specific queries.

**Pseudocode (Query Enhancement Phase):**

```
function enhanceQuery(searchQuery, userKnowledgeGraph) {
  candidatePhrases = identifyPhrases(searchQuery)

  for each phrase in candidatePhrases {
    graphNodes = mapPhraseToGraphNodes(phrase, userKnowledgeGraph) // Get nodes representing the phrase
    expansionCandidates = findConnectedNodes(graphNodes, userKnowledgeGraph) // Find connected nodes
    scoredCandidates = scoreExpansionCandidates(expansionCandidates, phrase, userKnowledgeGraph) // Score based on graph properties and relevance
    filteredCandidates = filterCandidates(scoredCandidates, threshold) // Filter based on score
    enhancedQuery = reformulateQuery(searchQuery, filteredCandidates) // Reformulate the query
  }

  return enhancedQuery
}
```

**Novelty:** This approach moves beyond static taxonomies by incorporating a dynamic, personalized knowledge graph based on the user’s *behavior*, leading to more relevant and tailored search results.  The temporal aspect (TTL for the graph) ensures it adapts to the user’s evolving interests.