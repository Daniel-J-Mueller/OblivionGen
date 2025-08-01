# 11281690

## Dynamic Data Masking via Federated Learning

**Concept:** Extend the dynamic connection string and credential management to incorporate a federated learning model for data masking. Instead of solely focusing on *where* the data resides and *who* is accessing it, we focus on *how* the data is presented to the client *based on observed access patterns and client sensitivity profiles*.

**Specs:**

*   **Component:**  Federated Learning Inference Server (FLIS)
*   **Input:** Connection String (as defined in the provided patent), Client Profile Data (sensitivity levels, role, device type), Raw Data (from the database).
*   **Output:**  Masked Data (redacted, anonymized, or pseudonymized).
*   **Architecture:**  FLIS sits *between* the database and the client application. It intercepts data requests, applies masking based on the federated learning model, and sends the masked data to the client.

**Federated Learning Model Details:**

*   **Model Type:**  Variational Autoencoder (VAE).  VAE allows for learning complex data distributions and generating realistic masked data.
*   **Training Data:**  Aggregated (and anonymized) query logs from all connected clients.  The query logs capture the types of data being requested.
*   **Federated Training:** The model is trained *locally* on each client’s anonymized query logs.  Only model updates (gradients) are sent to a central server for aggregation. This preserves data privacy.
*   **Personalization:** The aggregated model is then *personalized* on each client's device using a small amount of local data. This further enhances privacy and adapts masking to specific user needs.

**Pseudocode (FLIS Processing):**

```
function process_data_request(request, connection_string, client_profile):
  // 1. Establish connection to database using connection_string
  database_connection = connect_to_database(connection_string)

  // 2. Retrieve raw data based on request
  raw_data = execute_query(database_connection, request)

  // 3. Load personalized federated learning model for this client
  model = load_model(client_profile)

  // 4. Apply data masking using the model
  masked_data = apply_mask(model, raw_data)

  // 5. Return masked_data to client
  return masked_data
```

**System Integration:**

*   **Connection String Enhancement:** The connection string format is extended to include a pointer to the client's sensitivity profile (stored on a separate identity server).
*   **Connector Module Modification:** The connector module is responsible for retrieving the client profile and passing it to the FLIS.
*   **Database Audit Log:** All data masking operations are logged for audit purposes.

**Novelty:** This shifts the focus from securing data *at rest* and *in transit* to securing data *at presentation*. The federated learning approach ensures that masking is dynamic, personalized, and privacy-preserving. Existing dynamic connection string techniques only handle *where* the data comes from, not *how* it is presented. This adds an entirely new layer of security.