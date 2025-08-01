# 9201883

## Dynamic Package Fragmentation & Parallel Reconstruction

**Concept:** Instead of a single monolithic package file, split the package into numerous, smaller, independently addressable fragments. These fragments aren't simply sequential chunks; their content is determined by a rolling hash applied to the raw file data *during package creation*. Reconstruction isn’t linear; fragments are requested and assembled in parallel based on hash lookups, allowing for massive speed increases and inherent redundancy.

**Specifications:**

1.  **Package Creation (Fragmentation):**
    *   Input: Collection of raw files, desired fragment size (e.g., 1MB), hash function (e.g., SHA-256).
    *   Process:
        *   For each raw file:
            *   Divide the file into blocks.
            *   Calculate a rolling hash for each block.
            *   Based on the hash, assign the block to a fragment. Fragments are pre-allocated (number defined during package creation).
            *   Store block metadata (block ID, hash, fragment ID) in a central index file.
        *   Create fragment files.
        *   Create a master index file mapping fragment IDs to file locations.

2.  **Retrieval (Parallel Reconstruction):**
    *   Input: Request for a specific raw file.
    *   Process:
        *   Query the master index for the raw file.
        *   Identify all fragment IDs associated with the raw file's blocks.
        *   Issue parallel requests for all fragments.
        *   As fragments arrive:
            *   Verify hash of each block within the fragment.
            *   Reassemble blocks based on block ID.
            *   If a block is missing or hash verification fails, request it from an alternate source (e.g., redundant fragment storage, peer-to-peer network).
        *   Once all blocks are assembled, reconstruct the raw file.

**Data Structures:**

*   **Block Metadata:**
    *   `block_id`: Unique identifier for the block.
    *   `hash`: SHA-256 hash of the block data.
    *   `fragment_id`: ID of the fragment containing the block.
*   **Master Index:**
    *   `raw_file_id`: Unique identifier for the raw file.
    *   `block_list`: List of `block_id`s associated with the raw file.
*   **Fragment File:**
    *   A contiguous sequence of blocks.
    *   Metadata at the beginning indicating the fragment ID.

**Pseudocode (Retrieval):**

```
function retrieve_raw_file(raw_file_id):
  block_list = get_block_list(raw_file_id)
  fragment_ids = get_unique_fragment_ids(block_list)

  parallel_requests = []
  for fragment_id in fragment_ids:
    request = create_request(fragment_id)
    add_request_to_parallel_queue(request)

  reconstructed_data = ""
  while parallel_queue_not_empty():
    fragment_data = get_next_fragment_from_queue()
    for block in fragment_data.blocks:
      if block.block_id in block_list:
        if verify_hash(block.data, block.hash):
          reconstructed_data += block.data
        else:
          #Request Block from Alternate Source
          pass
  return reconstructed_data
```

**Novel Aspects:**

*   **Hash-based fragmentation**: Unlike simple sequential chunking, this method spreads data across fragments based on content, improving redundancy and parallelization potential.
*   **Parallel Reconstruction**: Fragments can be requested and assembled independently, drastically reducing retrieval time.
*   **Dynamic Redundancy**: Content distribution implicitly creates redundancy; loss of a fragment only impacts a portion of the files.

**Potential Applications**: Large-scale data archiving, content delivery networks (CDNs), distributed database systems.