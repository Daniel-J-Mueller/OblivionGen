# 11580145

## Dynamic Topic-Aware Vocabulary Expansion for Query Rephrasing

**Concept:** Expand the vocabulary available to the decoder LSTM not just with words *independent* of the initial topic, but with words dynamically sourced from a related knowledge graph, weighted by semantic similarity to the initial query. This allows for more nuanced and potentially more effective rephrasing, moving beyond simply swapping in unrelated terms.

**Specifications:**

1.  **Knowledge Graph Integration:**
    *   Establish a connection to a broad knowledge graph (e.g., ConceptNet, Wikidata).
    *   Upon receiving the initial query text data, extract key concepts. This can be achieved through Named Entity Recognition (NER) or keyword extraction techniques.
    *   Query the knowledge graph for concepts related to the extracted key concepts.
    *   Calculate a semantic similarity score between each retrieved knowledge graph concept and the original query. This can use techniques like word embeddings (Word2Vec, GloVe, BERT) and cosine similarity.

2.  **Dynamic Vocabulary Construction:**
    *   Create a candidate vocabulary expansion set by combining:
        *   Words from the original query (first subset).
        *   Words *absent* from the original query (second subset - as per the patent).
        *   A ranked selection of knowledge graph concepts. The ranking is determined by the semantic similarity score calculated in step 1.
    *   Apply a threshold to the semantic similarity score to limit the size of the expanded vocabulary.  Alternatively, implement a top-N selection (e.g., select the top 50 most similar concepts).

3.  **Decoder Integration:**
    *   The expanded vocabulary is used during the decoding process of the decoder LSTM.
    *   The attention mechanism (as described in claim 2) should be modified to consider the weights of the newly added knowledge graph concepts, potentially scaling them to prevent them from dominating the attention scores.

4.  **Rephrasing Strategy:**
    *   Introduce a "diversity penalty" during the decoding process. This encourages the decoder to explore different phrasing options and avoid simply repeating the original query with slightly different wording. This can be implemented by penalizing the selection of words that are highly similar to the words in the original query.

**Pseudocode (Decoder Update):**

```
function decode_step(decoder_hidden_state, encoder_output, expanded_vocabulary):
    # Calculate attention weights (as per patent claim 2)
    attention_vector = calculate_attention(decoder_hidden_state, encoder_output)

    # Calculate score vector for expanded vocabulary
    score_vector = calculate_score_vector(decoder_hidden_state, expanded_vocabulary)

    # Combine attention and score vectors
    combined_vector = concatenate(attention_vector, score_vector)

    # Calculate probability vector (softmax)
    probability_vector = softmax(combined_vector)

    # Sample next word based on probability vector
    next_word = sample_from_distribution(probability_vector)

    return next_word, decoder_hidden_state
```

**Additional Considerations:**

*   **Contextual Embeddings:** Utilize contextual word embeddings (e.g., BERT, RoBERTa) for both the original query and the knowledge graph concepts to capture more nuanced semantic relationships.
*   **Knowledge Graph Pruning:**  Implement a mechanism to prune irrelevant concepts from the knowledge graph based on the specific query topic.
*   **User Feedback Integration:** Allow users to provide feedback on the rephrased queries to further refine the knowledge graph and rephrasing strategies.