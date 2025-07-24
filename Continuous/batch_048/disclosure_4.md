# 9576299

## Dynamic Impression-Triggered Loyalty Points

**Concept:** Expand attribution beyond simple purchase correlation to create a dynamic loyalty point system triggered *by* impression exposure, rather than solely by purchase. This allows for rewarding customers for engaging with advertising, even if immediate purchase doesn't occur, fostering stronger brand interaction and data collection.

**Specs:**

*   **Data Inputs:**
    *   Impression Data: Timestamp, User ID (hashed/tokenized), Merchant ID, Ad Creative ID, Impression Provider Code.
    *   Transaction Data: Timestamp, User ID (hashed/tokenized), Merchant ID, Transaction Value, Payment Instrument.
    *   Loyalty Program Data: User ID (hashed/tokenized), Current Point Balance, Tier Level.
    *   ‘Engagement Weight’ Table:  A configurable table associating Ad Creative IDs with ‘Engagement Weights’ (values 0.0 - 1.0).  Higher weights indicate creatives designed to drive immediate engagement/action.  Weights can be adjusted based on A/B testing and performance metrics.
*   **System Components:**
    *   Impression Listener:  Receives impression data streams.
    *   Transaction Listener: Receives transaction data streams.
    *   Loyalty Engine:  Processes data, calculates points, updates loyalty accounts.
    *   Engagement Weight Manager:  Allows administrators to configure and update engagement weights.
    *   User ID Resolution Service:  Maps various user identifiers to a consistent hashed/tokenized ID.
*   **Workflow:**
    1.  Impression Listener receives impression data.
    2.  User ID Resolution Service resolves the User ID.
    3.  The system stores the impression event with associated User ID, timestamp, Merchant ID, and Engagement Weight (looked up from the Engagement Weight Table using Ad Creative ID).
    4.  Transaction Listener receives transaction data.
    5.  User ID Resolution Service resolves the User ID.
    6.  System searches for impressions associated with the User ID and Merchant ID within a configurable time window (e.g., 7 days).
    7.  For each matched impression:
        *   Calculate points earned: `Points = Transaction Value * Engagement Weight * Point Multiplier`.  `Point Multiplier` is a configurable setting.
        *   Add calculated points to the user's loyalty account.
    8.  If no transaction is found, points are still awarded based on impressions if an impression-triggered point threshold is met.
*   **Pseudocode:**

```
FUNCTION ProcessImpression(ImpressionData):
    UserID = ResolveUserID(ImpressionData.UserID)
    EngagementWeight = GetEngagementWeight(ImpressionData.AdCreativeID)
    StoreImpression(UserID, ImpressionData.Timestamp, ImpressionData.MerchantID, EngagementWeight)

FUNCTION ProcessTransaction(TransactionData):
    UserID = ResolveUserID(TransactionData.UserID)
    Impressions = GetImpressions(UserID, TransactionData.MerchantID, TransactionData.Timestamp)

    FOR EACH Impression IN Impressions:
        Points = TransactionData.TransactionValue * Impression.EngagementWeight * PointMultiplier
        UpdateLoyaltyAccount(UserID, Points)
```

*   **Configurable Parameters:**
    *   Point Multiplier
    *   Time Window for Impression Matching
    *   Impression-Triggered Point Threshold
    *   Engagement Weight Update Frequency
    *   User ID Resolution Logic