# 10296377

## Dynamic Job Fragment Granularity & Predictive Re-fragmentation

**Concept:** The existing patent describes dividing jobs into fragments based on a limited lifespan of compute instances. This expands upon that idea by introducing *dynamic* fragment sizing based on real-time compute instance load *and* predictive re-fragmentation based on historical processing data. Instead of static division, the system continuously assesses optimal fragment size and proactively re-fragments failing or overloaded fragments *before* their lifespan expires.

**Specifications:**

**1. Real-time Load Assessment Module:**

*   **Input:** Compute instance CPU/Memory utilization, Network I/O, Disk I/O, Historical fragment processing times.
*   **Processing:**  A continuously running module that monitors the load on each compute instance. It calculates a "Fragment Capacity Score" based on available resources.  This score dictates the *maximum* size a new fragment assigned to that instance can be.
*   **Output:** Fragment Capacity Score per compute instance.

**2. Fragment Sizing Algorithm:**

*   **Input:** Total job operations, Fragment Capacity Scores (from Real-time Load Assessment), Historical processing times for similar operations.
*   **Processing:**
    *   Initially divide the job into a baseline number of fragments.
    *   Dynamically adjust fragment sizes based on the Fragment Capacity Scores.  Instances with higher scores receive larger fragments.
    *   Employ a weighted average of historical processing times for similar operations to *estimate* the processing time of each fragment. Adjust fragment sizes to balance workload.
*   **Output:** List of job fragments with specified operation subsets and assigned compute instances.

**3. Predictive Re-fragmentation Engine:**

*   **Input:**  Real-time fragment processing progress, Historical data on fragment failures (operation type, instance ID, time of failure), Current fragment lifespan, Fragment Capacity Scores.
*   **Processing:**
    *   Utilize a machine learning model (e.g., LSTM or time series forecasting) to predict the probability of failure for each fragment based on its processing progress and historical data.
    *   If the predicted probability of failure exceeds a threshold *before* the fragment’s lifespan expires, proactively re-fragment the fragment into smaller sub-fragments.
    *   Distribute the sub-fragments to available compute instances, prioritizing those with higher Fragment Capacity Scores.
*   **Output:** List of new sub-fragments and assigned compute instances.

**4.  Fault Tolerance & Checkpointing:**

*   **Mechanism:** Each fragment will periodically checkpoint its progress to durable storage (e.g., cloud storage).
*   **Purpose:**  In case of instance failure, a new instance can resume processing from the last checkpoint, minimizing data loss and re-processing time.
*   **Trigger:** Checkpoints are triggered either based on a timer or based on a specific amount of progress.

**5. Pseudocode – Predictive Re-fragmentation Engine:**

```
function predict_fragment_failure(fragment_id, historical_data) {
  // Utilize machine learning model to predict probability of failure
  probability = ML_Model.predict(fragment_id, historical_data)
  return probability
}

function re_fragment_job(fragment_id, threshold) {
  if (predict_fragment_failure(fragment_id, historical_data) > threshold) {
    // Divide fragment into smaller sub-fragments
    sub_fragments = divide_fragment(fragment_id)

    // Distribute sub-fragments to available instances
    for each sub_fragment in sub_fragments {
      best_instance = find_best_instance(sub_fragment)
      assign_fragment(sub_fragment, best_instance)
    }
  }
}

function find_best_instance(fragment){
  //Return the compute instance with the highest Fragment Capacity Score,
  //and lowest current load.
}

//Main Loop
while (job_not_complete) {
  for each fragment in fragments {
    re_fragment_job(fragment.id, failure_threshold)
  }
  wait(monitoring_interval)
}

```

**Data Structures:**

*   `Fragment`:  `id`, `operations`, `status`, `assigned_instance`, `checkpoint_location`, `estimated_completion_time`.
*   `ComputeInstance`: `id`, `CPU_utilization`, `memory_utilization`, `Fragment_Capacity_Score`, `current_load`.