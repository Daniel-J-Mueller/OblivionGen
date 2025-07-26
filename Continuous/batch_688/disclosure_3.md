# 10733110

## Adaptive Prefetching Based on Transactional Metadata

**System Specifications:**

*   **Core Component:** Metadata Injection Module (MIM) - integrated within the memory controller.
*   **Data Structures:**
    *   *Transactional Metadata Table (TMT)*:  Stored in a fast, dedicated region of memory (SRAM or eDRAM). Indexed by physical address. Entries contain:
        *   Transaction Type (Read, Write, Erase, Mixed) - 8-bit enum.
        *   Data Locality Score (DLS) – 4-bit integer. Represents the spatial and temporal locality of recent accesses. Higher value = stronger locality.
        *   Prefetch Enable Flag – 1-bit boolean.  Determines whether prefetching is active for this block.
        *   Prefetch Distance – 4-bit integer.  Specifies how many blocks *ahead* to prefetch.
    *   *Prefetch Queue (PQ)*: FIFO queue managed by the memory controller. Holds prefetch requests.
*   **Operational Modes:**
    *   *Passive Mode:*  MIM monitors memory accesses, builds DLS, and learns access patterns. Prefetching is disabled.
    *   *Active Mode:* MIM analyzes DLS, determines optimal *Prefetch Distance*, enables prefetching, and populates PQ with requests.

**Innovation Description:**

The system dynamically adjusts prefetching behavior based on the *type* of transactions occurring on specific blocks of persistent memory. Existing prefetching algorithms are typically static or rely on coarse-grained heuristics. This innovation injects metadata about each transaction into the TMT. The MIM then uses this metadata to tailor prefetching strategies *at the block level*. 

For example:

*   **Sequential Writes:** High DLS. Aggressive prefetching with a large *Prefetch Distance*.
*   **Random Reads/Writes:** Low DLS. Minimal or no prefetching.
*   **Erase Operations:**  Special handling. Pre-fetch the block *after* the erase operation to prepare for subsequent writes.

**Pseudocode (MIM Operation):**

```pseudocode
// On each memory transaction (Read, Write, Erase):
function handleTransaction(physicalAddress, transactionType, dataSize):
  // Update TMT entry for physicalAddress
  tmtEntry = getTMTEntry(physicalAddress)

  if transactionType == WRITE:
    tmtEntry.transactionType = WRITE
    tmtEntry.DLS = calculateLocalityScore(physicalAddress, recentAccesses) //Spatial/Temporal
  else if transactionType == READ:
    tmtEntry.transactionType = READ
    tmtEntry.DLS = calculateLocalityScore(physicalAddress, recentAccesses)
  else if transactionType == ERASE:
    tmtEntry.transactionType = ERASE

  //Adaptive Prefetch Determination
  if tmtEntry.transactionType == WRITE and tmtEntry.DLS > threshold:
    tmtEntry.prefetchEnable = TRUE
    tmtEntry.prefetchDistance = determineOptimalDistance(tmtEntry.DLS)
    //Enqueue prefetch requests for the next 'prefetchDistance' blocks
    enqueuePrefetchRequests(physicalAddress + blockSize, tmtEntry.prefetchDistance)
  else:
    tmtEntry.prefetchEnable = FALSE

//Function to calculate locality score (spatial and temporal)
function calculateLocalityScore(physicalAddress, recentAccesses):
  //Analyze recent accesses to determine spatial and temporal locality
  //Return a score between 0 and 15 (for 4-bit integer)
  //Higher score indicates stronger locality

//Function to determine optimal prefetch distance based on locality score
function determineOptimalDistance(localityScore):
    //Map locality score to prefetch distance (0-15)
    //Higher locality = larger prefetch distance
    return mappingTable[localityScore]
```

**Hardware Considerations:**

*   Dedicated SRAM or eDRAM for the TMT.
*   Small, dedicated processing unit within the memory controller to handle MIM logic.
*   Potential for hardware acceleration of locality score calculation.
*   Integration with the existing memory controller’s prefetch engine.