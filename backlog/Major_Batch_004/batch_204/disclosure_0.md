# 11196748

## Dynamic Policy Mirroring & Federated Authentication

**Concept:** Extend the directory proxy concept to not only forward requests *and* credentials, but also to dynamically mirror relevant authentication/authorization policies from the on-premises network to the service provider network, creating a temporarily federated authentication environment.

**Specification:**

**1. Policy Mirroring Agent (PMA):** 
   - Deployed within the on-premises network (second network).
   - Monitors changes to authentication & authorization policies affecting resources accessible via the proxy.
   - Encapsulates policy changes into a standardized, lightweight format (Policy Delta).
   - Securely transmits Policy Delta to the service provider network via an established, authenticated channel (e.g., TLS with mutual authentication).

**2. Policy Integration Module (PIM):**
   - Deployed within the service provider network.
   - Receives Policy Delta from the PMA.
   - Parses Policy Delta and dynamically updates a “Shadow Policy Store” – a temporary, isolated copy of relevant policies.
   - Integrates Shadow Policy Store with the request evaluation process *before* forwarding to the on-premises network.

**3. Request Flow:**

   a. User attempts access to a resource. Request hits the service provider's directory proxy.
   b. Proxy obtains user credentials (as in the original patent).
   c. **NEW:** PIM intercepts request *before* forwarding.
   d. PIM evaluates request against Shadow Policy Store.
      *   If policy check passes, request is forwarded *with* original credentials.
      *   If policy check fails, access is denied *without* contacting on-premises network.
   e. On-premises network receives request and performs final authorization (redundancy check).
   f. Response is returned.

**4.  Pseudocode (PIM – Policy Evaluation):**

```
function evaluateRequest(request, credentials):
  policyDelta = receivePolicyDelta() //From PMA
  updateShadowPolicyStore(policyDelta)

  // Retrieve policies applicable to the requested resource from ShadowPolicyStore
  applicablePolicies = ShadowPolicyStore.getPolicies(request.resource)

  //Evaluate request against applicable policies
  if (evaluate(request, credentials, applicablePolicies)):
    return TRUE // Policy check passed
  else:
    return FALSE // Policy check failed
```

**5.  Key Features:**

   *   **Reduced Latency:** Policy checks performed within the service provider network, minimizing round trips to the on-premises network.
   *   **Increased Security:** Allows granular access control without exposing sensitive on-premises policies to the service provider.
   *   **Dynamic Adaptation:**  Policy updates propagated in near real-time, ensuring consistency.
   *   **Federated Trust:** Creates a temporary, localized federation of authentication & authorization.
   *   **Compatibility:** Designed to work alongside existing directory services and authentication protocols.



**6.  Considerations:**

   *   **Policy Conflict Resolution:** Mechanism to handle conflicting policies between on-premises and service provider environments.
   *   **Policy Expiration:**  Policies mirrored from on-premises should have defined expiration times to ensure they remain current.
   *   **Scalability:**  PIM must be able to handle a large volume of policy updates and requests.
   *  **Audit Logging:** All policy updates and evaluation results must be logged for auditing and troubleshooting purposes.