# 11706706

## Dynamic AP ‘Personality’ Profiles & Predictive Steering

**Concept:** Expand beyond simple load balancing/band steering based on disconnect reasons and capability *reporting*. Create a system that actively *learns* an AP’s ‘personality’—its tendency to behave in specific ways under varying conditions—and proactively steers devices *before* disconnects occur, based on predicted behavior.

**Specs:**

1.  **Data Collection Enhancement:** Expand data collection beyond disconnect reasons, BSSID, OUI, and capability *reports*. Add the following real-time metrics from wireless devices:
    *   **Signal Strength Fluctuation:** Standard deviation of signal strength over a 5-second window.
    *   **Packet Loss Rate (PLR):**  PLR experienced *to* the AP, calculated at the device.
    *   **Latency Jitter:** Variation in round-trip time (RTT) to the AP.
    *   **Device ‘Happiness’ Score:** A weighted combination of the above, reflecting device experience. (Weights configurable via ML).

2.  **AP ‘Personality’ Model:**  Implement a recurrent neural network (RNN), specifically a Long Short-Term Memory (LSTM) network, for *each* AP. This model will ingest the time-series data (from step 1) from connected devices.

    *   **Input:** Time-series data from connected devices (Signal Strength Fluctuation, PLR, Latency Jitter, Device ‘Happiness’ Score).
    *   **Output:** A vector representing the AP’s current ‘state’ or ‘personality’—essentially, a prediction of its short-term behavior.  This vector can include metrics like:
        *   Probability of causing a disconnect within the next X seconds.
        *   Expected PLR for new connections.
        *   Predicted latency jitter.

3.  **Predictive Steering Engine:**
    *   When a device scans for available APs, the steering engine requests the ‘personality’ vector from each candidate AP’s LSTM model.
    *   The engine calculates a ‘steering score’ for each AP, weighting the personality vector metrics based on device requirements (e.g., a video streaming device prioritizes low latency, a file transfer device prioritizes throughput).
    *   The device is steered to the AP with the highest steering score, *before* any connection is established.

4.  **Model Training:**
    *   The LSTM models are continuously trained using data collected from all connected devices.
    *   A centralized server aggregates data and updates the models periodically.
    *   Federated Learning could be implemented to train models locally on each AP, enhancing privacy.

**Pseudocode (Steering Engine):**

```
function steerDevice(device, scanResults):
  for each ap in scanResults:
    personalityVector = getPersonalityVector(ap)
    steeringScore = calculateSteeringScore(device, ap, personalityVector)
    ap.steeringScore = steeringScore

  bestAp = findMax(ap.steeringScore)
  connectToAp(device, bestAp)
```

```
function calculateSteeringScore(device, ap, personalityVector):
  weight_latency = device.latency_weight
  weight_throughput = device.throughput_weight
  score = (weight_latency * (1 - personalityVector.predicted_latency_jitter)) + (weight_throughput * personalityVector.predicted_throughput)
  return score
```

**Novelty:** This goes beyond reactive steering based on disconnects and capability reporting. It *proactively* predicts AP behavior and steers devices accordingly, aiming to *prevent* poor user experience before it happens.  The use of LSTM networks allows for modeling time-dependent behavior, offering a more nuanced understanding of AP performance.