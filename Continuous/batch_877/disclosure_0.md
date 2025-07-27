# 10877769

## Adaptive Granularity Thread Scheduling

**Concept:** The current patent focuses on 1:1 mapping of application threads to execution threads. This design proposes a system where the granularity of thread assignment is *dynamic*, shifting between coarse-grained (multiple application threads mapped to a single execution thread) and fine-grained (1:1 mapping) based on workload characteristics and execution thread utilization.

**Specs:**

*   **Component:** Adaptive Scheduler Module (ASM) – residing on the GPU Server.
*   **Inputs:**
    *   Command stream from Computing Device (as per the original patent).
    *   Execution Thread Utilization Data – real-time monitoring of each execution thread’s workload (percentage of time actively processing commands).
    *   Application Thread Metadata – profiling data indicating the typical workload characteristics of each application thread (e.g., compute-intensive, I/O bound, frequency of blocking calls). This metadata could be pre-calculated and sent alongside commands, or dynamically learned.
*   **Outputs:**
    *   Dynamically adjusted thread assignments – mapping of application threads to execution threads.
    *   Workload distribution adjustments – instructions to the execution threads regarding their assigned workload.

**Algorithm (Pseudocode):**

```pseudocode
// Initialization:
threadCount = number of execution threads
applicationThreadCount = number of application threads
initialMapping = round(applicationThreadCount / threadCount) // Basic distribution

currentMapping = initialMapping
utilizationData = [] // Array to store utilization of each execution thread

// Main Loop (executed continuously on GPU Server):
for each command in commandStream:
    applicationThreadID = command.applicationThreadID
    
    // Determine target execution thread:
    targetExecutionThreadID = applicationThreadID % threadCount
    
    // Monitor execution thread utilization:
    utilizationData[targetExecutionThreadID] = getExecutionThreadUtilization(targetExecutionThreadID)
    
    // Adaptive adjustment logic:
    if (utilizationData[targetExecutionThreadID] > thresholdHigh):
        // Thread is overloaded. Attempt to redistribute workload.
        // Identify underutilized threads
        underutilizedThread = findUnderutilizedThread(utilizationData)
        if (underutilizedThread != null):
            // Redirect future commands for applicationThreadID to underutilizedThread
            reassignApplicationThread(applicationThreadID, underutilizedThread)
    else if (utilizationData[targetExecutionThreadID] < thresholdLow):
        // Thread is underutilized. Attempt to consolidate workload.
        // Identify overloaded threads
        overloadedThread = findOverloadedThread(utilizationData)
        if (overloadedThread != null):
            // Migrate some commands from overloadedThread to this thread
            migrateCommands(overloadedThread, targetExecutionThreadID)
            
    // Assign command to execution thread
    assignCommandToExecutionThread(command, targetExecutionThreadID)
```

**Data Structures:**

*   `ThreadAssignmentMap`:  A mapping of application thread IDs to execution thread IDs.
*   `UtilizationData`: An array storing the utilization percentage of each execution thread.
*   `CommandQueue`:  Per-execution thread queue for incoming commands.

**Implementation Notes:**

*   `thresholdHigh` and `thresholdLow` need to be tuned based on hardware and workload characteristics.
*   Workload migration should be carefully managed to avoid excessive data transfer overhead.
*   The algorithm can be extended to consider thread priority and affinity.
*   The system should provide metrics for monitoring the effectiveness of the adaptive scheduling algorithm. This may require extensive testing.