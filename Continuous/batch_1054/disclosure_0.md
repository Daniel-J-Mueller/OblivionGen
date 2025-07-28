# 9880960

## Dynamic State Register Allocation for Multi-Tenant Security

**Concept:** Extend the configurable sponge function engine to support dynamic allocation of the state register's bitrate and capacity sections *per virtual machine or tenant*. This enables a security architecture where the "strength" of the sponge function (defined by bitrate/capacity) is tailored to the security needs and resource allocation of each tenant, improving overall system security and efficiency.

**Specifications:**

*   **Hardware:** Integrated within a System-on-Chip (SoC) or FPGA, alongside a virtualization engine (hypervisor).
*   **State Register Modification:** The state register is logically partitioned.  A "meta-controller" module manages these partitions.
*   **Meta-Controller:**
    *   Receives security policies/resource allocations from the virtualization engine for each VM/tenant. These policies define minimum bitrate, maximum bitrate, minimum capacity, maximum capacity.
    *   Dynamically adjusts the bitrate/capacity section sizes within the state register to meet these policies. This is performed *without* halting the sponge function processing.
    *   Includes a "safety net" mechanism: if a tenant attempts to allocate more bitrate/capacity than available, the meta-controller defaults to a pre-defined minimum or throws an exception handled by the virtualization engine.
*   **Configuration Signals:** New control signals are added to the existing controller:
    *   `tenant_id[N]`: Identifies the current tenant/VM.
    *   `bitrate_alloc[M]`: Specifies the bitrate allocation for the current tenant.
    *   `capacity_alloc[P]`: Specifies the capacity allocation for the current tenant.
*   **Modified Configurable Message Processor:** The message processor now receives `tenant_id`, `bitrate_alloc`, and `capacity_alloc` signals *in addition* to the standard bitrate size indication. The fragment size is determined based on the tenant’s allocation.
*   **Iterative Calculator:**  No changes required. The calculator operates on whatever bitrate/capacity is configured by the meta-controller.
*   **Output Size Adaptor:** No changes required. 
*   **Security Considerations:**
    *   The meta-controller itself *must* be highly secure. Access should be tightly controlled by the hypervisor.
    *   Mechanisms to prevent "bitrate starvation" (one tenant consuming all available bitrate) are required.

**Pseudocode (Meta-Controller):**

```
//Initialization
state_register_size = total_register_size // Fixed hardware constraint

//Per-VM Configuration
function configure_vm(vm_id, bitrate_request, capacity_request) {
  if (bitrate_request + capacity_request > state_register_size) {
    //Handle error: insufficient resources.  Default to minimum or throw exception
    bitrate_alloc[vm_id] = min_bitrate
    capacity_alloc[vm_id] = min_capacity
    return
  }

  bitrate_alloc[vm_id] = bitrate_request
  capacity_alloc[vm_id] = capacity_request

  //Signal Configurable Message Processor with new allocation values
  send_config(vm_id, bitrate_alloc[vm_id], capacity_alloc[vm_id])
}

//Main Loop (triggered by VM context switch)
function on_vm_switch(vm_id) {
  //Retrieve allocation values for the current VM
  bitrate = bitrate_alloc[vm_id]
  capacity = capacity_alloc[vm_id]

  //Signal Configurable Message Processor
  send_config(vm_id, bitrate, capacity)
}
```

**Potential Use Cases:**

*   Multi-tenant cloud security: Tailor sponge function strength to customer tiers.
*   IoT device security: Allocate more security resources to critical devices.
*   Secure enclaves: Isolate and protect sensitive data with customized sponge functions.