# 10692116

## Dynamic Impression Value (DIV) System

**Concept:** The existing patent focuses on price points *before* an impression is served. This system introduces a dynamic impression value (DIV) that *changes* during the impression lifecycle based on real-time user engagement. Instead of a fixed bid winning, the impression ‘earns’ its value.

**Specs:**

*   **Component Integration:** Requires modifications to the Ad Exchange Server, Mobile Device, and potentially the Second Ad Server (Third Party).
*   **Engagement Metrics:** DIV calculation factors include:
    *   *Viewability:* Percentage of impression visible on screen (standard metric).
    *   *Dwell Time:* Duration user actively views the ad (measured in seconds).
    *   *Interaction:* Any user interaction with the ad (clicks, video plays, form submissions, etc.).
    *   *User Attention Score:*  (Novel) Utilizing mobile device sensors (camera – with user permission, accelerometer, gyroscope) to estimate user attention.  If the user is looking at the screen, the score increases. If the device is moved abruptly or the screen is off, the score decreases. This is a weighted score.
*   **DIV Calculation:**
    *   Initial DIV = Winning Bid Amount.
    *   DIV Update Frequency: Every 0.5-1 second.
    *   DIV = Initial DIV + (Viewability Weight \* Viewability Percentage) + (Dwell Time Weight \* Dwell Time in Seconds) + (Interaction Weight \* Interaction Score) + (Attention Score Weight \* Attention Score).  Weights are configurable by the publisher/advertiser.
*   **Real-Time Revenue Share:**  The ad exchange server calculates the DIV in real-time. A percentage of the DIV is continuously distributed to the publisher and the winning advertiser. This percentage is configurable.
*   **Impression ‘Expiration’:** If the DIV falls below a predefined threshold (e.g., due to low engagement or user attention), the impression is considered ‘expired’ and the ad is removed/replaced (if possible). This prevents paying for impressions nobody is seeing or engaging with.
*   **Mobile Device Implementation:**  SDK integrated into the mobile application.  The SDK collects engagement metrics and transmits them to the Ad Exchange Server.  Handles user permission requests for sensor data.
*   **Ad Exchange Server Modifications:**
    *   New API endpoints for receiving engagement metrics from mobile devices.
    *   DIV calculation logic.
    *   Real-time revenue distribution logic.
    *   Impression expiration handling.
    *   Data storage for engagement metrics and DIV history (for reporting/analytics).
*   **Second Ad Server (Third Party) Modifications:**  Receives updated revenue share information from the Ad Exchange Server.

**Pseudocode (Ad Exchange Server - DIV Calculation):**

```
function calculateDIV(engagementMetrics, winningBidAmount):
  viewabilityWeight = getConfig("viewabilityWeight")
  dwellTimeWeight = getConfig("dwellTimeWeight")
  interactionWeight = getConfig("interactionWeight")
  attentionScoreWeight = getConfig("attentionScoreWeight")

  viewabilityPercentage = engagementMetrics.viewability
  dwellTime = engagementMetrics.dwellTime
  interactionScore = engagementMetrics.interactionScore
  attentionScore = engagementMetrics.attentionScore

  div = winningBidAmount + (viewabilityWeight * viewabilityPercentage) + (dwellTimeWeight * dwellTime) + (interactionWeight * interactionScore) + (attentionScoreWeight * attentionScore)

  return div
```

**Potential Benefits:**

*   **Increased Revenue:**  Higher revenue for publishers and advertisers by rewarding engaged impressions.
*   **Improved Ad Quality:** Incentivizes serving ads that users actually engage with.
*   **Enhanced Transparency:**  Provides a more accurate measure of ad value.
*   **Real-time Optimization:**  Allows for real-time optimization of ad campaigns based on user engagement.