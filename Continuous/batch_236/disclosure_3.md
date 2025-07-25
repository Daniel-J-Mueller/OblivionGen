# 10033613

## Dynamic Flow Sampling with Predictive Burst Detection

**Concept:** Augment the existing historical flow tracking with a dynamic sampling mechanism that *predicts* potential bursts based on short-term flow behavior, and proactively increases sampling rates for those flows *before* they become overwhelming. This contrasts with purely reactive approaches.

**Specs:**

*   **Component:** Predictive Sampling Module (PSM). This exists as an additional processing stage *before* the existing key generation and memory read stages.
*   **Input:** Raw packet information (identical to the existing input).
*   **Data Structures:**
    *   *Short-Term Flow Table (STFT):*  A memory table (RAM) keyed by flow ID. Each entry contains:
        *   *Packet Rate (PR):*  Packets/second observed in the last N seconds (N configurable – default 1 second).
        *   *Delta Rate (DR):* Change in PR over the last M seconds (M < N – default 0.5 seconds).
        *   *Prediction Confidence (PC):* A value (0-100) reflecting the algorithm’s certainty of a potential burst.
    *   *Burst Threshold Table (BTT):* A configurable table mapping flow types (e.g., HTTP, SSH, DNS) to base burst thresholds (packets/second).  Flow type determined through deep packet inspection (DPI) – existing capability assumed.

*   **Algorithm (PSM):**
    1.  **Flow Identification:** Extract flow ID from packet information.
    2.  **STFT Lookup:**
        *   If flow ID exists in STFT:
            *   Update PR based on packet arrival time.
            *   Calculate DR.
        *   If flow ID does not exist:
            *   Create new entry in STFT with initial PR=1 (arbitrary value) and DR=0.
    3.  **Prediction Calculation:**
        *   Calculate a Prediction Score (PS) using the following formula: `PS = DR * (1 + (PR / BTT[FlowType]))` .  The `BTT[FlowType]` acts as a scaling factor based on the expected baseline rate for that flow type.
        *   Update PC.  Algorithm: `PC = min(100, PC + (PS * K))` where K is a learning rate constant (configurable – default 0.1).  This linearly increases confidence based on the prediction score.  The `min` function clamps PC to 100.
    4.  **Sampling Rate Adjustment:**
        *   If PC > Threshold (configurable - default 70): Increase sampling rate for this flow. A tiered approach is preferred:
            *   70 < PC < 85: Sample every other packet.
            *   85 < PC < 95: Sample every packet.
            *   PC > 95:  Sample all packets + insert a “high priority” flag.
        *   Otherwise: Use default sampling rate (based on existing hardware limitations).
    5.  **Output:** Packet information + sampling rate/priority flag. This flag is passed to the key generation logic, potentially influencing how the key is generated to ensure sampled packets receive priority in memory access.

*   **Integration:** The PSM sits between the raw packet input and the existing key generation logic.  The sampling rate/priority flag is an addition to the packet information.
*   **Memory Requirements:**  The STFT will require RAM.  Size dependent on maximum number of concurrent flows and the configured retention period (N and M). This is a trade-off.
*   **Performance Considerations:** The PSM adds processing overhead.  Optimization required to ensure it doesn't become a bottleneck.  Hardware acceleration (e.g., using FPGA) could be explored.
*   **Data Collection & Reporting:**  The PSM should log the number of flows that had their sampling rates adjusted, as well as the average increase in sampled packets. This provides feedback on the effectiveness of the predictive sampling mechanism.