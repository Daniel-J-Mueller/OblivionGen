# 10489289

## Adaptive Spatial Granularity for Trim & Data Journaling

**Concept:** Dynamically adjust the spatial granularity of both trim propagation *and* data journaling based on observed workload patterns and SSD characteristics. The core idea is to move beyond fixed-size clusters/blocks and leverage sub-block (or even bit-level) granularity where appropriate to minimize write amplification and maximize storage efficiency.

**Specifications:**

**1. Granularity Levels:**

*   **Coarse:** Standard cluster/block-level journaling and trim (as in the provided patent). Baseline for compatibility and simplicity.
*   **Medium:** Sub-block (e.g., 1/4, 1/8) granularity.  Partition blocks into smaller, manageable segments for independent journaling/trim.
*   **Fine:** Bit-level granularity. Track trim/dirty bits *within* blocks using a dedicated metadata structure.  Facilitates highly precise trim and eliminates wasted space.

**2. Metadata Structure: Granularity Map**

*   A dedicated metadata structure (the “Granularity Map”) associates each logical block (or range of blocks) with its current granularity level.  This map itself is stored on the SSD and is fault-tolerant (e.g., redundant copies, checksums).
*   Granularity Map entries include:
    *   Logical Block Address Range
    *   Current Granularity Level (Coarse, Medium, Fine)
    *   Associated Metadata Pointers (for Medium/Fine levels - pointers to segment metadata or bit-level maps).
*   The Granularity Map is updated asynchronously to avoid impacting write performance.

**3. Adaptive Granularity Engine:**

*   A background process (the “Adaptive Granularity Engine”) monitors SSD workload characteristics:
    *   Write patterns (sequential vs. random).
    *   Trim frequency.
    *   SSD internal wear leveling.
    *   Block erase counts.
*   Based on these metrics, the engine dynamically adjusts the granularity level for logical blocks.
    *   High write amplification & frequent trims: transition to finer granularity.
    *   Sequential writes & low trim frequency: transition to coarser granularity.
    *   Wear leveling data: prioritize fine granularity for frequently rewritten blocks.

**4. Journaling Implementation:**

*   Journal entries are dynamically sized based on the current granularity level.
*   Coarse: Standard block-level journal.
*   Medium/Fine:  Journal entries track only the *modified* segments/bits within a block, significantly reducing journal size.  Each entry includes a range marker indicating the beginning and ending segment/bit.

**5. Trim Propagation:**

*   Trim requests are processed at the requested granularity (host-driven).
*   The system maps host-level trims down to the internal granularity level, updating the metadata structures accordingly.
*   Fine-level trim propagates directly to the bit-level map.
*   Medium/Fine: Trim is recorded as a range marker within the segment metadata.

**Pseudocode (Adaptive Granularity Engine):**

```
function analyze_workload()
    read SSD metrics (write amplification, trim frequency, wear leveling)
    calculate workload score
    return workload score

function adjust_granularity(logical_block_address)
    workload_score = analyze_workload()

    if workload_score > threshold_high:
        set_granularity(logical_block_address, FINE)
    else if workload_score < threshold_low:
        set_granularity(logical_block_address, COARSE)
    else:
        set_granularity(logical_block_address, MEDIUM)

function set_granularity(logical_block_address, granularity_level)
    update Granularity Map entry for logical_block_address
    if granularity_level == FINE:
        allocate bit-level map for the block
    else if granularity_level == MEDIUM:
        allocate segment metadata for the block
```

**Potential Benefits:**

*   Reduced write amplification.
*   Improved storage efficiency.
*   Increased SSD lifespan.
*   Adaptive to different workloads.
*   Fine-grained control over trim propagation.