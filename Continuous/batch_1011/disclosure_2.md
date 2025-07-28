# 10296377

**Adaptive Job Fragment Granularity & Predictive Rescheduling**

**Concept:** The existing patent discusses dividing jobs into fragments processed by time-bound compute instances. This design expands upon that by dynamically adjusting fragment size *during* job execution and proactively rescheduling fragments based on predicted instance failure. 

**Specifications:**

*   **Module:** ‘Granularity Manager’ – a core component integrated into the existing computing resource service.
*   **Input:**
    *   Batch job definition (list of operations).
    *   Real-time performance data from compute instances (CPU load, memory usage, network latency, past failure rates).
    *   Historical data on operation execution times (collected over multiple job runs).
    *   Defined ‘stability score’ thresholds for compute instances.
*   **Data Structures:**
    *   `Operation List`: A list of operations comprising the batch job. Each operation tagged with estimated execution time and resource requirements.
    *   `Fragment`:  A contiguous subset of operations from the `Operation List`.  Includes a start index, end index, assigned compute instance ID, and current status.
    *   `Instance Health Record`: Stores real-time and historical performance data for each compute instance, including a calculated ‘stability score’.
*   **Algorithm:**

    1.  **Initial Fragmentation:** The batch job is initially divided into fragments based on pre-defined criteria (e.g., maximum fragment duration, resource limits).

    2.  **Dynamic Granularity Adjustment:**
        *   The ‘Granularity Manager’ continuously monitors the performance of compute instances processing fragments.
        *   If a compute instance consistently finishes fragments *much* faster than estimated, the fragment size assigned to that instance is *increased* dynamically.
        *   If a compute instance consistently struggles to complete fragments within the time limit, the fragment size is *decreased*.
        *   Adjustments are made incrementally to avoid disruption.  A ‘fragment resizing factor’ controls the maximum adjustment per iteration.
    3.  **Predictive Rescheduling:**
        *   The ‘Granularity Manager’ uses a ‘failure prediction model’ (trained on historical data) to estimate the probability of failure for each compute instance.
        *   If the predicted failure probability exceeds a defined threshold, any fragments currently assigned to that instance are *proactively* rescheduled to healthy instances *before* the failure occurs.
        *   Rescheduling prioritizes fragments with critical dependencies or those nearing completion.
    4.  **Feedback Loop:** The actual execution times of fragments are used to refine the estimation of operation execution times, improving the accuracy of future fragmentation and rescheduling decisions.
*   **Pseudocode – Predictive Rescheduling Function:**

    ```
    function predictAndReschedule(instanceID):
      failureProbability = calculateFailureProbability(instanceID)
      if failureProbability > FAILURE_THRESHOLD:
        fragmentsToReschedule = getFragmentsAssignedTo(instanceID)
        for fragment in fragmentsToReschedule:
          findHealthyInstance(fragment) // Find available instance
          transferFragment(fragment, healthyInstance) // Move fragment to new instance
          updateFragmentStatus(fragment, "Rescheduled")
      return
    ```

*   **API Endpoints:**
    *   `/fragmentation/config`:  Allows adjustment of fragmentation parameters (maximum fragment duration, resizing factor, failure threshold).
    *   `/instance/health/{instanceID}`: Returns the health record for a specific instance.

*   **Deployment Considerations:** Requires integration with existing monitoring and logging infrastructure.  The ‘failure prediction model’ needs to be periodically retrained with updated data.