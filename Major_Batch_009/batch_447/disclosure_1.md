# 10187289

## Dynamic Route Tag Inheritance & Propagation Profiles

**Concept:** Extend the tag-based routing system to support *inheritance* of tags based on network topology and pre-defined propagation profiles, allowing for finer-grained control and automated tag application based on route origin and destination. This moves beyond simple tag application and propagation to a system where tags are dynamically adjusted *during* transit.

**Specifications:**

**1. Propagation Profile Database:**

*   **Data Structure:**  Key-value store.
    *   **Key:**  Combination of:
        *   Origin AS (Autonomous System)
        *   Route Type (e.g., IPv4, IPv6)
        *   Initial Tag Value (if present)
    *   **Value:** Propagation Profile – A list of rules.  Each rule defines:
        *   **Condition:** A network topology condition (e.g., “If route traverses AS X,” “If route enters region Y,” “If route's next hop is device Z”)
        *   **Action:** A tag manipulation operation:
            *   **Add Tag:** Append a new tag value.
            *   **Remove Tag:** Delete a specified tag value.
            *   **Replace Tag:** Replace a specific tag value with a new one.
            *   **Inherit Tag:** Copy a tag from the traversing interface/device.
            *   **Conditional Add:** Add a tag *only* if a specific attribute of the route (e.g., community string, AS path length) meets a criteria.

**2.  Endpoint Router Tag Application Logic:**

*   **Initial Tagging:**  When a route is received *from* the client network, determine the initial tag based on client-configured policies (e.g., based on service level agreement).
*   **Propagation Profile Lookup:** When forwarding a route, the endpoint router consults the Propagation Profile Database, using the origin AS and any existing tags as lookup keys.
*   **Dynamic Tag Manipulation:** Apply the actions defined in the matching Propagation Profile rule(s) to the route’s tag list.

**3.  Interface/Device Tag Inheritance:**

*   Each network interface/device maintains a list of “inherited tags”.
*   When a route traverses an interface/device, the interface/device can *add* its inherited tags to the route’s tag list (configurable per interface/device). This allows for tagging based on physical location or administrative domain.

**4. Tag Prioritization & Conflict Resolution:**

*   Define a tag prioritization scheme (e.g., highest tag value, tag assigned by highest authority AS).
*   Implement conflict resolution logic to handle conflicting tag assignments.

**Pseudocode (Endpoint Router Forwarding Logic):**

```pseudocode
function forwardRoute(route):
  originAS = route.originAS
  tags = route.tags
  
  # Lookup Propagation Profile
  profile = lookupPropagationProfile(originAS, tags)

  # Apply Profile Rules
  for rule in profile.rules:
    if rule.condition is met:
      applyTagAction(route, rule.action)

  # Inherit Tags from Interface (if configured)
  if interface.inheritTags:
    route.tags.addAll(interface.inheritedTags)

  # Forward Route
  forwardRouteToNextHop(route)
```

**Example Scenario:**

*   Client A in AS 100 wants to advertise a route.
*   The endpoint router tags the route with "ClientA_RegionX".
*   The route traverses a region with a special security policy.
*   The Propagation Profile adds a "Security_High" tag.
*   The route reaches a data center with limited bandwidth.
*   The Propagation Profile adds a "Bandwidth_Limited" tag.
*   The final route advertisement includes tags "ClientA_RegionX", "Security_High", and "Bandwidth_Limited", reflecting the path it took and the policies it encountered.