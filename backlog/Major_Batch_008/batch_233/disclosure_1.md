# 9385887

## Dynamic Virtual Network 'Chaining'

**Concept:** Extend the virtual network mapping beyond simple protocol translation to create dynamically chained virtual networks for tiered security and functionality. Instead of just mapping protocols between a virtual network and the external network, allow virtual networks to *nest* within each other, with traffic flowing through multiple layers of virtualized services before reaching its final destination.

**Specs:**

*   **Component:** *Virtual Network Chain Manager (VNC Manager)* – A centralized controller responsible for establishing and managing the chains. Operates independently of the existing virtual network mapping programs.
*   **Data Structure:** *Chain Definition File* – A JSON or YAML file defining the sequence of virtual networks, the services each network provides (e.g., firewall, intrusion detection, data analytics), and the routing rules between them.
*   **Interface:** *Chain Request API* – An API allowing applications or network administrators to request a specific chain for their traffic. Includes parameters for security level, performance requirements, and desired services.
*   **Protocol:** *Chain Control Protocol (CCP)* – A lightweight protocol used by the VNC Manager to configure and monitor the virtual network nodes within the chain. Utilizes gRPC for efficient communication.

**Operation:**

1.  An application requests a chain via the Chain Request API, specifying its requirements.
2.  The VNC Manager consults the Chain Definition Files and selects an appropriate chain.
3.  The VNC Manager dynamically provisions the virtual network nodes in the chain, allocating resources as needed.
4.  The VNC Manager configures the nodes using the CCP, setting up routing rules and service configurations.
5.  The application’s traffic is redirected to the first node in the chain.
6.  Each node processes the traffic according to its configuration and forwards it to the next node in the chain.
7.  The final node forwards the traffic to its intended destination.

**Pseudocode (VNC Manager – Chain Creation):**

```
function create_chain(request_data):
  chain_definition = select_chain(request_data.security_level, request_data.performance_requirements)

  if chain_definition is null:
    return error("No suitable chain found")

  chain_nodes = []
  for node_config in chain_definition.nodes:
    new_node = provision_virtual_network_node(node_config.image, node_config.resources)
    chain_nodes.append(new_node)

  for i in range(len(chain_nodes) - 1):
    configure_routing(chain_nodes[i], chain_nodes[i+1])

  return chain_nodes[0] # Return the entry point to the chain
```

**Novelty:**

This differs from the original patent’s focus on protocol translation by introducing the concept of *composed* virtual networks. Instead of merely translating between protocols, this system creates dynamic, multi-layered virtual networks that can provide advanced security and functionality. The ability to chain virtual networks on demand allows for flexible and scalable network services. This can also be applied to load balancing across multiple virtual networks.