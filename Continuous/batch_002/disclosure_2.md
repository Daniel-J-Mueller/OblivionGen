# 9935816

**Autonomous System (AS) Path Diversity via Stochastic Routing**

**Concept:** Extend the ability to manipulate AS paths beyond simply allowing a customer ASN to appear twice. Introduce *stochastic* AS path selection, injecting controlled randomness into BGP routing decisions to enhance network resilience and potentially reduce reliance on single best-path calculations.

**Specifications:**

*   **Component:** BGP Route Diversification Module (BRDM) - a software component integrated into virtual gateway/router systems.
*   **Input:** Standard BGP routing updates.  Customer ASN, Service Provider ASN, configured diversity level (0-100, representing probability of path diversification).
*   **Process:**
    1.  Receive standard BGP update.
    2.  Identify Customer ASN and Service Provider ASN.
    3.  Determine if diversity level is > 0.
    4.  If diversity level > 0:
        *   Generate a random number between 0 and 100.
        *   If the random number is <= the configured diversity level:
            *   Modify the AS Path:  Instead of simply adding the customer ASN twice, inject a controlled number of additional, valid ASNs *before* the customer ASN. These injected ASNs could be selected from a pre-defined, geographically diverse pool. This pool is configurable.
            *   Calculate a new BGP path attribute based on the injected ASNs (e.g., a 'diversity score').
            *   Advertise the modified route with the new attribute.
        *   Else:  Advertise the standard route.
*   **Output:** Modified or standard BGP routing updates.
*   **Configuration Parameters:**
    *   Diversity Level (0-100) – Probability of path diversification.
    *   Injected ASN Pool – List of valid ASNs to inject into the AS path.  Configurable by geographic region.
    *   Diversity Score Algorithm – Method for calculating the diversity score based on injected ASNs.
    *   Monitoring/Logging – Detailed logging of diversified routes and associated metrics.

**Pseudocode:**

```
function processBGPUpdate(bgpUpdate):
  customerASN = bgpUpdate.customerASN
  serviceProviderASN = bgpUpdate.serviceProviderASN
  diversityLevel = getConfigParameter("diversityLevel")

  if diversityLevel > 0:
    randomNum = generateRandomNumber(0, 100)

    if randomNum <= diversityLevel:
      injectedASNPool = getConfigParameter("injectedASNPool")
      randomInjectedASN = selectRandomASN(injectedASNPool)

      modifiedASPath = bgpUpdate.ASPath + [randomInjectedASN] + [customerASN]
      diversityScore = calculateDiversityScore(modifiedASPath)

      bgpUpdate.ASPath = modifiedASPath
      bgpUpdate.diversityScore = diversityScore

  return bgpUpdate
```

**Rationale:**

Current methods focus on duplicating ASNs for basic redundancy. This approach introduces minimal path diversity. By injecting a configurable pool of additional ASNs, the system dynamically increases path diversity, creating multiple viable routes. This enhanced diversity improves network resilience against link failures or congestion and potentially reduces the reliance on single, optimal paths.  The configurable diversity level allows administrators to fine-tune the balance between path diversity and routing complexity. The diversity score enables BGP to intelligently select paths based on diversity as well as traditional metrics.