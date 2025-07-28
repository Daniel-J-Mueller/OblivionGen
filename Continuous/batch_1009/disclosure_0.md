# 8826270

## Dynamic Memory Affinity Mapping

**Concept:** Augment the CPU scheduling with a dynamic memory affinity mapping system. Instead of purely prioritizing VMs based on priority/credits/cache misses, analyze *where* in physical memory each VM’s working set resides and proactively migrate VMs to CPUs with NUMA nodes closest to their primary memory locations.

**Specification:**

1.  **Memory Footprint Tracking:** A hypervisor component monitors each VM's active memory pages. This isn’t a full memory copy, but metadata tracking – which physical memory addresses are actively referenced by the VM. We can use existing hardware performance counters for page access tracking.

2.  **NUMA Node Identification:** For each VM, identify the NUMA node(s) where the majority of its active memory pages reside. Maintain a weighted average, considering access frequency.

3.  **CPU Affinity Scoring:** Develop a scoring system that assigns a “proximity score” to each CPU based on its distance (in NUMA hops) from the VM’s primary memory location(s).  Lower hop count = higher score.

4.  **Scheduling Integration:** The CPU scheduler incorporates the proximity score alongside existing priority/credit/cache miss metrics. VMs with higher proximity scores are given preferential scheduling on CPUs with corresponding high scores.

5.  **Dynamic Migration:** Implement a lightweight VM migration mechanism. When a VM's primary memory location shifts significantly (detected by the memory footprint tracker), initiate a migration to a CPU closer to the new location. Migration should be minimally disruptive – using techniques like dirty page tracking and incremental copying.

6.  **Adaptive Thresholds:** The sensitivity of the memory footprint tracker and the thresholds for triggering migration should be dynamically adjusted based on system load and memory contention.

**Pseudocode (Scheduler Integration):**

```
function schedule_next_vm() {
  best_vm = null
  best_score = -1

  for each vm in running_vms {
    priority_score = vm.priority
    credit_score = vm.credits
    cache_miss_penalty = calculate_cache_miss_penalty(vm)
    proximity_score = calculate_proximity_score(vm) //Based on memory location

    total_score = priority_score + credit_score - cache_miss_penalty + proximity_score

    if (total_score > best_score) {
      best_score = total_score
      best_vm = vm
    }
  }

  return best_vm
}

function calculate_proximity_score(vm) {
  //Determine NUMA node of VM's primary memory
  primary_node = get_vm_primary_node(vm)

  //Score based on distance to CPU's NUMA node
  proximity = 0
  for each cpu in available_cpus {
    if (cpu.numa_node == primary_node) {
      proximity = 10 //Max score
      break;
    }
  }
  return proximity
}
```

**Hardware Requirements:**

*   Support for hardware performance counters for memory access tracking.
*   NUMA-aware CPU architecture.

**Potential Benefits:**

*   Reduced memory latency and improved overall system performance.
*   More efficient use of memory bandwidth.
*   Reduced contention for memory resources.
*   Improved VM isolation and security.