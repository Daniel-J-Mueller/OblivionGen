# 11216500

**Dynamic Email ‘Constellations’ – Visualizing Relationship Networks**

**Specification:**

**I. Core Concept:**  Expand the existing mailbox view organization beyond keyword/frequency/people. Introduce a dynamically generated visual ‘constellation’ of emails, where each email is a node, and relationships between emails (based on shared entities – people, places, organizations, topics) are represented as connecting lines.  The strength/thickness of the line indicates the degree of relationship.  

**II. Data Structures:**

*   `EmailNode`:  Represents a single email.  Attributes: `emailID`, `subject`, `sender`, `date`, `bodyText`, `entityList` (list of identified entities – people, organizations, locations, keywords).
*   `RelationshipEdge`: Represents a connection between two `EmailNode` objects. Attributes: `node1ID`, `node2ID`, `weight` (representing the strength of the relationship – calculated based on shared entities, co-occurrence in time, etc.).
*   `ConstellationGraph`: A graph data structure containing a collection of `EmailNode` and `RelationshipEdge` objects.

**III. Algorithm:**

1.  **Entity Extraction:** Upon receiving a set of search results (as per the provided patent), perform Named Entity Recognition (NER) on the body text of each email to populate the `entityList` for each `EmailNode`.
2.  **Relationship Calculation:** For each pair of emails, calculate a `weight` based on the overlap in their `entityList`.  A simple formula could be: `weight =  |entityList1 ∩ entityList2| / |entityList1 ∪ entityList2|`.   More sophisticated weighting could consider the *type* of entity (e.g., a shared person is more significant than a shared keyword), the time difference between the emails (closer emails have stronger connections), and the frequency of shared entities.
3.  **Graph Construction:** Create a `ConstellationGraph` by adding the emails as nodes and the calculated relationships as edges.
4.  **Visualization:**  Render the `ConstellationGraph` as an interactive visualization.  Users should be able to:
    *   Pan and zoom the visualization.
    *   Click on a node to view the full email.
    *   Adjust the strength threshold for displaying connections (to declutter the visualization).
    *   Filter the visualization by entity type (e.g., show only connections based on people).
    *   Apply layouts such as force-directed graphs or hierarchical radial trees to emphasize clusters of related emails.
5.  **Dynamic Updates:** As new emails arrive or search criteria change, dynamically update the `ConstellationGraph` and visualization in real-time.

**IV.  Implementation Details:**

*   **NER Library:**  Utilize a pre-trained NER library (e.g., SpaCy, Stanford NLP) for entity extraction.
*   **Graph Visualization Library:**  Employ a graph visualization library (e.g., D3.js, vis.js) to render the `ConstellationGraph` in the user interface.
*   **Performance Optimization:**  Implement caching and lazy loading to handle large email datasets efficiently. Consider using WebGL for rendering the graph visualization if performance becomes a bottleneck.

**V.  User Interface Elements:**

*   A toggle to activate/deactivate the ‘Constellation View’
*   A slider to adjust the relationship strength threshold
*   A dropdown menu to filter by entity type
*   A button to automatically rearrange the constellation based on a chosen layout algorithm.