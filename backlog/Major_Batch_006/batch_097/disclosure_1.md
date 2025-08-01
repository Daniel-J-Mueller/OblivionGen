# 11537431

## Dynamic Task Affinity & Predictive Pre-Fetch

**Concept:** Extend the task distribution system to learn worker affinity for specific task *types* (not just resource cells) and proactively pre-fetch tasks based on predicted needs, minimizing latency and maximizing throughput.

**Specifications:**

**1. Task Type Metadata:**

*   Each task is tagged with a ‘Task Type’ identifier (e.g., “SnapshotCreate,” “SnapshotDelete,” “SnapshotCopy,” “SnapshotView”, “SnapshotShare”).  This is a new field added to the existing task data structure.
*   Maintain a 'Task Type Profile' for each worker. This profile is a vector representing the historical performance (completion time, resource usage) for each Task Type on that worker.

**2. Affinity Scoring:**

*   Introduce an “Affinity Score” calculated periodically (e.g., every 5 minutes) for each worker/Task Type pair.
*   Affinity Score =  `WeightedAverage(HistoricalPerformanceMetrics)`
    *   Metrics: Completion Time (inverse relationship - lower is better), Resource Utilization (lower is better), Error Rate (inverse relationship - lower is better).
    *   Weights: Configurable per Task Type, allowing prioritization of metrics (e.g., prioritize low latency for SnapshotView).
*   The Task Service maintains a global “Affinity Map” storing Affinity Scores for all workers and Task Types.

**3. Predictive Pre-Fetch:**

*   The Task Service monitors task request patterns over time.  It identifies frequently occurring Task Type sequences (e.g., SnapshotCreate -> SnapshotView -> SnapshotCopy).
*   Based on observed patterns and worker Affinity Scores, the Task Service proactively pre-fetches tasks for workers.
*   Pre-fetch buffer: Each worker has a limited-size pre-fetch buffer. The Task Service populates this buffer with likely next tasks based on predictive modeling.
*   Pre-fetch validation: Before adding a task to the buffer, the Task Service checks if the worker has sufficient resources to handle it.

**4. Token Enhancement:**

*   Extend the existing token to include:
    *   ‘Last Task Type’ completed by the worker.
    *   ‘Prediction Confidence’ score –  A value representing the Task Service’s confidence in its next task prediction.

**5. Worker Logic Modification:**

*   Workers prioritize tasks from the pre-fetch buffer if available.
*   Workers send the updated token (including Last Task Type and Prediction Confidence) with each request.
*   Workers can optionally signal to the Task Service if a pre-fetched task is not suitable (e.g., due to unexpected data conditions). This feedback is used to refine the predictive model.

**Pseudocode (Task Service - Task Assignment):**

```
function AssignTask(worker, token):
  taskType = token.LastTaskType
  affinityScores = GetAffinityScores(worker, taskType)
  predictedTasks = PredictNextTasks(worker, affinityScores)

  if predictedTasks.size > 0 and worker.resourcesAvailable():
    task = predictedTasks.get(0)
    UpdateToken(token, task.TaskType) //Update LastTaskType for next iteration
    return task

  //Default Task Selection Logic (as in existing system)
  task = SelectTaskFromKeySpace(token)
  return task

function PredictNextTasks(worker, affinityScores):
  //Analyze historical task sequences and worker affinity
  //Return a prioritized list of likely next tasks
  //Consider worker capacity and resource availability

function UpdateToken(token, taskType):
  token.LastTaskType = taskType
```

**Data Structures:**

*   **Task Type Profile:** `Map<TaskType, Map<Metric, Value>>`
*   **Affinity Map:** `Map<WorkerID, Map<TaskType, Score>>`
*   **Pre-Fetch Buffer:**  `Queue<Task>` (limited size)