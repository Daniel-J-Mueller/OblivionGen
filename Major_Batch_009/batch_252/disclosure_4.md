# 8250473

## Dynamic Heatmaps of User Attention

**Concept:** Expand the user path visualization into a dynamic heatmap representing *attention* within files, not just transitions *between* them. This builds on the idea of tracking user movement but adds a layer of engagement detail.

**Specs:**

**1. Data Acquisition & Processing:**

*   **Input:** Network resource logs (as in the original patent) *plus* detailed user interaction data within files. This includes:
    *   Mouse movement tracking (coordinates over time).
    *   Scrolling behavior (scroll position over time).
    *   Time spent on specific elements (e.g., images, text blocks – requires DOM analysis or similar element identification).
    *   Keystrokes (optional – for text-heavy content, can indicate areas of focus).
*   **Processing:**
    *   Divide each file/webpage into a grid (e.g., 10x10, configurable).
    *   For each grid cell, calculate an “attention score” based on the collected user interaction data. Score calculation should factor in:
        *   *Density of mouse movements:* More movements = higher score.
        *   *Scroll depth:* Cells visible during deep scrolling receive higher scores.
        *   *Time spent:* Longer gaze/interaction time = higher score.
        *   *Keystroke density* (if available).
    *   Aggregate attention scores over all users, weighted by time. This creates a heatmap for each file.
    *   Store heatmaps as image data or numerical arrays associated with the original file.

**2. Visualization Engine:**

*   **Heatmap Overlay:** When a user path is visualized, overlay the corresponding heatmap onto the file representation.
    *   Use a color gradient (e.g., blue to red) to represent attention intensity.  Red indicates high attention, blue low attention.
    *   Allow configurable heatmap transparency to balance path visualization with attention data.
*   **Dynamic Heatmap Updates:**  Heatmaps should update in real-time as new user interaction data arrives. This provides a live view of user attention.
*   **Aggregation Levels:**
    *   *Individual User Heatmaps:* Allow viewing attention for a single user.
    *   *Cohort Heatmaps:* Aggregate heatmaps for specific user groups (e.g., by demographics, behavior).
    *   *Overall Heatmap:* Display the average attention across all users.
*   **Interactive Exploration:**
    *   *Hover/Click:**  When the user hovers over or clicks on a grid cell in the heatmap, display detailed interaction data (e.g., number of mouse movements, time spent).
    *   *Zoom/Pan:*  Allow zooming and panning within the heatmap to explore areas of high attention in more detail.

**3. System Architecture:**

*   **Data Pipeline:**  Network logs and interaction data feed into a data processing pipeline (e.g., using Apache Kafka, Apache Spark) for real-time calculation of attention scores and heatmap generation.
*   **Data Storage:**  Heatmap data (image data or numerical arrays) and interaction data are stored in a scalable database (e.g., Cassandra, MongoDB).
*   **Visualization Server:** A dedicated visualization server renders the user paths and heatmaps based on data retrieved from the database.
*   **API:**  A REST API allows access to heatmap data and interaction data for external applications.

**Pseudocode (Heatmap Calculation):**

```
function calculate_heatmap(file_id, interaction_data):
    grid = divide_file_into_grid(file_id)
    heatmap = initialize_heatmap(grid)

    for interaction in interaction_data:
        grid_cell = find_grid_cell_for_coordinates(interaction.x, interaction.y)
        heatmap[grid_cell] += interaction.weight  // Weight based on interaction type/duration

    normalize_heatmap(heatmap)  // Scale values to a 0-1 range
    return heatmap
```