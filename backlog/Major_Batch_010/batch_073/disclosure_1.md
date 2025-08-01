# 10154039

## Dynamic Access Control Propagation with Temporal Decay

**Concept:** Extend the hierarchical access control by introducing a ‘decay’ factor to permissions over time. This acknowledges that access needs change, and prevents stale permissions from indefinitely blocking legitimate access.  Instead of static inheritance, permissions ‘leak’ down the hierarchy, diminishing with each level and over time, requiring periodic re-assertion or re-evaluation.

**Specification:**

**1. Data Structures:**

*   `AccessControlEntry (ACE)`:  (Resource, User/Group, Permission, ExpirationTimestamp, DecayRate)
    *   `Resource`:  Identifier of the controlled resource.
    *   `User/Group`: Identifier of the entity granted/denied access.
    *   `Permission`:  Type of access (Read, Write, Execute, etc.).
    *   `ExpirationTimestamp`: Absolute timestamp after which the ACE is considered invalid.
    *   `DecayRate`:  A floating-point value (0.0 – 1.0) representing the rate at which the permission's ‘strength’ diminishes over time.  Higher values mean faster decay.

*   `HierarchyAccessMap`: A mapping of resource identifiers to lists of `ACE` entries, including inherited (decayed) permissions.

**2. Algorithm: `PropagateAccessControl(Resource, User)`**

```pseudocode
function PropagateAccessControl(Resource, User):
  HierarchyAccessMap = GetHierarchyAccessMap(Resource) //Retrieve existing map or create new
  ParentResources = GetParentResources(Resource) //Get immediate parents
  
  for each ParentResource in ParentResources:
    ParentACEs = GetAccessControlEntries(ParentResource, User)
    
    for each ParentACE in ParentACEs:
      EffectivePermission = CalculateEffectivePermission(ParentACE, Resource, User)
      
      if EffectivePermission != DENIED:
        AddACEtoHierarchyAccessMap(Resource, User, EffectivePermission)
        
  //Determine Final Effective Permissions after propagation/decay
  FinalPermissions = CalculateFinalPermissions(Resource, User)
  return FinalPermissions

function CalculateEffectivePermission(ParentACE, Resource, User):
    TimeSinceGranted = CurrentTime - ParentACE.ExpirationTimestamp
    DecayedPermission = ParentACE.Permission * (1 - TimeSinceGranted * ParentACE.DecayRate)
    
    if DecayedPermission < MINIMUM_PERMISSION_THRESHOLD:
        return DENIED // Permission effectively expired
    else:
        return DecayedPermission

function CalculateFinalPermissions(Resource, User):
    //Aggregate all permissions (inherited and direct) for Resource/User.
    //Apply any conflict resolution logic. (e.g., DENY overrides ALLOW)
    //Return final effective permission set.
```

**3. System Components:**

*   **Access Control Manager:**  Handles all access control requests, invoking `PropagateAccessControl` as needed.  Caches `HierarchyAccessMap` for performance.
*   **Decay Engine:** A background process that periodically evaluates the `HierarchyAccessMap`. It identifies ACEs with decayed permissions (below the `MINIMUM_PERMISSION_THRESHOLD`) and flags them for removal or re-evaluation.
*   **Policy Editor:**  Allows administrators to define access control policies, including `DecayRate` values for different permissions and resource types.

**4. Integration with Existing System:**

*   Extend the existing `GetParentResources` function to include all ancestors in the resource hierarchy.
*   Modify the existing permission evaluation logic to incorporate the decayed permission values.
*   Add a configuration parameter to enable/disable the decay mechanism.

**5.  Novelty:** This differs from standard hierarchical access control by dynamically adjusting permissions based on time and a configurable decay rate. This provides a more flexible and adaptive security model, reducing the need for constant manual policy updates while still providing strong access control.  It acknowledges the changing nature of access requirements and promotes a ‘least privilege’ approach over time.