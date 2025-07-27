# 10275789

## Dynamic Impression Weighting via Biometric Correlation

**Concept:** Enhance attribution accuracy by incorporating real-time biometric data from the user device to weight the influence of online impressions. This goes beyond simple matching of identifiers and attempts to establish a *correlation* between biometric state during impression exposure and subsequent in-store purchase.

**Specs:**

*   **Data Acquisition:** User device (smartphone, smartwatch) passively collects biometric data: heart rate, skin conductance (GSR), eye tracking (pupil dilation, gaze direction). Data collection is opt-in and anonymized/pseudonymized.
*   **Impression Tagging:**  Each served online impression is tagged with a timestamp and associated data (merchant ID, product ID, location).  Simultaneously, the biometric data stream from the user device is recorded with corresponding timestamps.
*   **Biometric Baseline Establishment:**  A personalized biometric baseline is established for each user based on their typical biometric readings during normal activity (not during ad exposure).  This helps differentiate between genuine engagement and random fluctuations.
*   **Engagement Scoring:** When an impression is displayed, the current biometric data is compared to the user's baseline.  Metrics are calculated:
    *   *Heart Rate Variance (HRV):*  Increase or decrease relative to baseline.
    *   *GSR Activation:*  Change in skin conductance as a proxy for emotional arousal.
    *   *Gaze Dwell Time:* Duration of focused eye tracking on the impression content.
    *   *Pupil Dilation:* Measured change in pupil size, indicative of cognitive load.
    These metrics are combined into a weighted ‘Engagement Score’ (ES).  Weights can be adjusted via machine learning.
*   **Transaction Matching:** As in the original patent, transaction data (customer identifier, merchant ID, timestamp, product ID) is received.
*   **Attribution Algorithm:** The attribution algorithm now incorporates ES:
    *   A time window is defined (e.g., 30 minutes, 1 hour) before the transaction.
    *   All impressions within the time window are identified.
    *   Each impression’s Engagement Score is multiplied by a “Weight Factor” (WF) - initially 1.0.
    *   A “Weighted Score” is calculated for each impression (Engagement Score \* WF).
    *   The impression with the highest Weighted Score is considered the primary attribution source.
*   **Feedback Loop & Weight Factor Adjustment:**
    *   If a transaction *does not* occur within a reasonable timeframe (e.g., 7 days) after an impression with a high Weighted Score, the Weight Factor for impressions from that merchant is *reduced* slightly.
    *   If a transaction *does* occur shortly after an impression with a high Weighted Score, the Weight Factor for impressions from that merchant is *increased* slightly.
    *   This iterative process refines the attribution model over time.

**Pseudocode (Simplified Attribution):**

```
FUNCTION AttributeTransaction(transactionData, impressions):

  filteredImpressions = FilterImpressionsByTimeWindow(impressions, transactionData.timestamp)

  FOR each impression IN filteredImpressions:
    impression.weightedScore = impression.engagementScore * impression.weightFactor

  primaryImpression = FindMax(filteredImpressions, impression.weightedScore)

  IF primaryImpression != NULL:
    AttributeTransactionToImpression(transactionData, primaryImpression)
    AdjustWeightFactor(primaryImpression.merchantId, transactionData.timestamp)

  END FUNCTION
```

**Hardware Requirements:**

*   User device with biometric sensors (heart rate, GSR, eye tracking).
*   Secure data transmission and storage infrastructure.
*   Machine learning platform for weight factor adjustment.