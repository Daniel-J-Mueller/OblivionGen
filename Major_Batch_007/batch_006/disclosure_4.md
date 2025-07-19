# 8165923

**Personalized Catalog "Time Capsule"**

**Concept:** Extend the personalized catalog experience beyond simply displaying *what* a user previously ordered, to present a dynamic “time capsule” of their purchasing history *within* the category page. This isn't just showing the item, but *when* they ordered it, how it related to events *around* that time, and potentially even external data linked to that purchase.

**Specs:**

*   **Data Source:** Integrate with user calendar data (with permission), social media activity (optional, with explicit permission), and publicly available event data (news, sports scores, weather).
*   **Category Page Integration:**  When a user views a category page, the system identifies prior purchases within that category. Instead of simply listing the item, it presents a visual timeline.
*   **Timeline Visualization:** The timeline displays:
    *   Date of the order.
    *   Item purchased.
    *   A small "context card" displaying events happening *around* that date (e.g., "Local team won championship game," "User had a calendar event: Vacation to Hawaii," "Major news event: First image of black hole released").  This information is sourced from the integrated data sources.
    *   Optionally, a snippet of the user's social media activity from around that time (if permission granted).
    *   A "Memory Lane" button allowing users to view a full-screen expanded timeline of all purchases in that category.
*   **Recommendation Engine Enhancement:**  The timeline data feeds into the recommendation engine.  Purchases paired with specific events or contextual data get higher weight. (e.g. If a user bought hiking boots right before a vacation, recommendations for hiking accessories are boosted).
*   **Privacy Controls:**  Robust privacy settings are crucial. Users must explicitly opt-in to data integration and have granular control over what information is displayed.

**Pseudocode:**

```
function generate_category_page(user_id, category_id):
  orders = get_orders_by_user_and_category(user_id, category_id)
  timeline_events = []

  for order in orders:
    event = {
      "date": order.order_date,
      "item": order.item_name,
      "context": get_contextual_events(order.order_date) //Call to external API/data source
    }
    timeline_events.append(event)

  category_page = render_category_page(category_data, timeline_events)
  return category_page
```

**Data Structures:**

*   `Order`:  `{user_id, order_date, item_name, category_id}`
*   `ContextEvent`: `{date, description, source}`
*   `TimelineEvent`: `{date, item_name, context: ContextEvent}`