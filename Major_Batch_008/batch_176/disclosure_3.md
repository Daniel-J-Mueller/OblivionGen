# 12079734

## Adaptive Subgraph Parallelization

**Concept:** Extend the subgraph identification and optimization approach to dynamically parallelize subgraph execution based on real-time compute/memory resource availability on the target processor *during* inference. The initial patent focuses on *compile-time* decisions, this focuses on *runtime* adaptation.

**Specifications:**

1.  **Runtime Profiling Module:** A module integrated into the inference engine continuously monitors the target processor’s compute units (CPU/GPU cores, specialized accelerators) and memory bandwidth/capacity usage. This module generates a 'resource availability vector' representing current capacity.

2.  **Dynamic Subgraph Dependency Graph (DSDG):**  Instead of a static dependency graph created at compile time, build a DSDG that’s updated during inference. Nodes represent subgraphs. Edges represent data dependencies. The DSDG also includes estimated compute and memory requirements for each subgraph.

3.  **Subraph Partitioning & Scheduling:**
    *   The DSDG is partitioned into executable 'chunks'. A chunk is a set of independent subgraphs that can be run concurrently.
    *   A scheduler uses the resource availability vector and DSDG to decide which chunks to execute. Prioritization rules:
        *   **Memory-Bound Chunks First:** If memory bandwidth is the limiting factor, prioritize subgraphs that consume less memory.
        *   **Compute-Bound Chunks First:** If compute is the limiting factor, prioritize subgraphs with high compute demands.
        *   **Hybrid Scheduling:**  Balance memory and compute usage based on the resource availability vector.
        *   **Dynamic Chunk Size:** The size of executable chunks adapts based on available resources. Larger chunks when resources are plentiful, smaller chunks when resources are constrained.

4.  **Data Buffering & Communication:** Implement a buffering system to manage data flow between subgraphs. Use asynchronous communication to prevent blocking. Buffer size is dynamically adjusted based on subgraph data requirements and memory availability.

5.  **Adaptive Precision Scaling:** Integrate automatic mixed precision (AMP) and dynamic precision scaling into the subgraph execution. Based on resource availability, adjust the precision of operations within subgraphs. Lower precision for compute-intensive subgraphs, higher precision for critical subgraphs.

**Pseudocode (Scheduler):**

```
function schedule_subgraphs(DSDG, resource_availability):
  executable_chunks = find_independent_chunks(DSDG)
  sorted_chunks = sort_chunks_by_resource_usage(executable_chunks, resource_availability)

  for chunk in sorted_chunks:
    if can_execute_chunk(chunk, resource_availability):
      execute_chunk(chunk)
      update_resource_availability(resource_availability, chunk)
    else:
      delay_chunk(chunk) //Queue to retry later
      
function can_execute_chunk(chunk, resource_availability):
  //Check if combined memory and compute requirements of the chunk
  //are within available resource limits.
  return (chunk.memory_usage < resource_availability.memory_bandwidth) and
         (chunk.compute_usage < resource_availability.compute_capacity)

function update_resource_availability(resource_availability, chunk):
  resource_availability.memory_bandwidth -= chunk.memory_usage
  resource_availability.compute_capacity -= chunk.compute_usage
```

**Hardware Considerations:**

*   Requires a processor with sufficient compute cores and memory bandwidth.
*   Benefit from specialized accelerators (e.g., NPUs) to accelerate subgraph execution.
*   Memory hierarchy optimization is crucial for minimizing data transfer latency.