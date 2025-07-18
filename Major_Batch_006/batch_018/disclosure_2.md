# 10963497

## Dynamic Query Decomposition & Predictive Entity Linking

**Concept:** Extend the multi-stage query processing to *decompose* complex queries into sub-queries based on predicted relationships *before* initial intent classification. Simultaneously, proactively link potential entities across these sub-queries, creating a knowledge graph fragment representing the user's overall need.

**Rationale:** The current system appears to classify intent *then* extract entities. A user's query may contain multiple interwoven intents, better addressed by breaking it down and treating it as a small "investigation". Pre-decomposition and linking allows for a more holistic understanding and potentially avoids misinterpretations stemming from early intent commitment.

**System Specs:**

1.  **Decomposition Engine:**
    *   Input: Raw text query.
    *   Process: Utilize a transformer-based model (e.g., a modified BART or T5) fine-tuned on a dataset of complex queries and their optimal sub-query decompositions. The model outputs a set of sub-queries, each representing a distinct facet of the original request.  Sub-query creation prioritizes semantic independence – minimizing overlap in information need.
    *   Output: A list of sub-queries. Each sub-query includes a confidence score indicating the model's certainty about its relevance.

2.  **Entity Linking & Graph Construction:**
    *   Input: Raw text query + list of sub-queries.
    *   Process:
        *   Run Named Entity Recognition (NER) on the original query *and* each sub-query independently.
        *   Apply a coreference resolution algorithm to identify mentions of the same entity across all queries.
        *   Construct a preliminary knowledge graph fragment:
            *   Nodes: Entities identified through NER & coreference.
            *   Edges: Relationships inferred from the query structure (e.g., "subject-verb-object" relationships within sub-queries, connections based on common entities). Edge weights represent confidence scores.

3.  **Intent Classification (Distributed):**
    *   Input: Each sub-query + associated entity graph fragment.
    *   Process: A suite of intent classifiers (as in the base system) is applied *to each sub-query independently*.  The entity graph fragment serves as contextual input to the classifiers, informing their decision-making.
    *   Output:  A list of intent classifications, one per sub-query, along with confidence scores.

4.  **Knowledge Base Integration & Response Synthesis:**
    *   Input: List of sub-query intents, associated entity graph, and confidence scores.
    *   Process: 
        *   Formulate individual KB queries for each sub-query based on the identified intent and entities.
        *   Aggregate results from the KB, resolving any inconsistencies or conflicts based on the entity graph's edge weights and confidence scores.
        *   Synthesize a coherent response addressing the original, complex query.  The synthesis process prioritizes information presented in the order indicated by the entity graph's structure (representing the user's implied logical flow).

**Pseudocode (KB Integration/Response Synthesis):**

```pseudocode
function synthesize_response(subquery_intents, entity_graph, kb_results):
  ordered_subqueries = topological_sort(entity_graph) // Order based on dependencies
  response = ""
  for subquery in ordered_subqueries:
    intent = subquery_intents[subquery]
    kb_result = kb_results[subquery]
    // Format kb_result based on intent (e.g., answer, list, description)
    formatted_result = format_result(kb_result, intent)
    response += formatted_result + " "
  return response
```

**Novelty:** The dynamic decomposition and proactive entity linking create a more adaptable and nuanced query processing pipeline.  It moves beyond treating queries as monolithic requests, embracing the inherent complexity and interrelationships within user needs.  The system can better handle ambiguous or multifaceted questions, delivering responses that are more relevant, coherent, and comprehensive. This differs from the base patent which appears to focus on sequential intent determination and entity extraction.