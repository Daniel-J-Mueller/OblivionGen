# 10810662

## Adaptive Granularity Time-Series with Predictive Archival

**Concept:** Expand upon the time-series archival concept by introducing *adaptive granularity* and *predictive archival* based on transaction patterns. Instead of fixed time periods (weekly, etc.), the system dynamically adjusts the granularity of time-series data based on transaction density and predicted future activity. This minimizes storage costs while maintaining query performance.

**Specifications:**

**1. Data Structures:**

*   **Transaction Metadata Store:** (Key-Value Store) Stores metadata for each transaction *beyond* the amount and updated value. Includes:
    *   `transaction_id`: Unique identifier.
    *   `user_id`: Associated user.
    *   `timestamp`: Precise timestamp of the transaction.
    *   `transaction_type`: (e.g., credit, debit, transfer).
    *   `transaction_tags`: (Arbitrary tags for categorization - optional).
*   **Time-Series Data Store:** (Columnar Database optimized for time-series) – Stores actual balance data at varying granularities.
    *   `time_bucket_start`: Start time of the data bucket.
    *   `time_bucket_end`: End time of the data bucket.
    *   `granularity`: (e.g., second, minute, hour, day, week).
    *   `balance`: Current balance at the end of the time bucket.
    *   `transaction_count`: Number of transactions in the bucket.
*   **Prediction Model Store:** Stores trained machine learning models (e.g., LSTM, Prophet) for predicting transaction volume per user/account.

**2. System Components:**

*   **Transaction Ingestion Service:** Receives transaction requests, performs idempotency checks (as in the patent), and writes to the Transaction Metadata Store.
*   **Granularity Adjustment Engine:** (Core Component)
    *   Analyzes transaction metadata from the Transaction Metadata Store *in real-time*.
    *   Uses a sliding window to calculate transaction density per user/account.
    *   Queries the Prediction Model Store to obtain predicted transaction volume for the next time period.
    *   Determines the optimal time bucket duration (granularity) based on a cost-benefit analysis (storage cost vs. query performance).  High transaction density = smaller buckets (more granular).  Low density = larger buckets.
*   **Archival Process:** (Enhanced from patent)
    *   Asynchronously archives data into time-series buckets with the dynamically determined granularity.
    *   Includes a compaction process to merge smaller buckets into larger ones when transaction density decreases.
    *   Prioritizes archiving older data to minimize storage costs.
*   **Query Engine:**
    *   Automatically selects the appropriate time-series buckets based on the query time range.
    *   Handles queries across multiple buckets with different granularities.
    *   Supports aggregation and filtering of transaction data.

**3. Pseudocode - Granularity Adjustment Engine:**

```pseudocode
function adjust_granularity(user_id, current_time):
  // 1. Retrieve recent transaction data
  recent_transactions = get_transactions(user_id, current_time - 1 hour, current_time)
  transaction_density = calculate_transaction_density(recent_transactions)

  // 2. Load prediction model
  prediction_model = load_prediction_model(user_id)

  // 3. Predict future transaction volume
  predicted_volume = predict_transaction_volume(prediction_model, current_time + 1 hour, current_time + 24 hours)

  // 4. Determine optimal granularity
  if transaction_density > HIGH_THRESHOLD and predicted_volume > HIGH_PREDICTION:
    granularity = SECOND
  elif transaction_density > MEDIUM_THRESHOLD and predicted_volume > MEDIUM_PREDICTION:
    granularity = MINUTE
  elif transaction_density > LOW_THRESHOLD and predicted_volume > LOW_PREDICTION:
    granularity = HOUR
  else:
    granularity = DAY

  return granularity
```

**4. New Claims (Example):**

1.  A system comprising: at least one computing device; at least one data store; an application executable by the at least one computing device, wherein the application, when executed, causes the at least one computing device to: receive a request to update a balance; determine a dynamic granularity for storing the balance update based on a real-time analysis of transaction density and a prediction of future transaction volume; and archive the balance update into a time-series data store using the determined dynamic granularity.

2.  The system of claim 1, wherein the application employs a machine learning model to predict future transaction volume.

3.  The system of claim 1, wherein the dynamic granularity is adjusted independently for each user account.