# 11573816

## Dynamic Task-Aware Image Composition

**Concept:** Extend prefetching to not just individual images, but *composable* images – images built from layers or base images that can be dynamically assembled on the compute resource *before* task execution, minimizing transfer size and maximizing resource utilization.

**Specifications:**

**1. Image Manifests:**

*   Introduce a new manifest format, “Composition Manifests” alongside existing image manifests.
*   Composition Manifests define a Directed Acyclic Graph (DAG) of image layers or base images required for a task. Each node in the DAG represents an image layer/base, and edges define dependencies.
*   Each node includes a reference to a standard image registry and a unique layer identifier.
*   The manifest includes a "cost" attribute for each layer based on size, potential CPU usage during assembly, and network cost.

**2. Assembly Service:**

*   Introduce an "Assembly Service" running within each cluster (potentially co-located with the container service).
*   The Assembly Service is responsible for:
    *   Receiving Composition Manifests from the Container Service.
    *   Identifying missing layers based on the cache contents of the compute resource.
    *   Downloading only the missing layers from the image registry.
    *   Assembling the final container image layer-by-layer on the compute resource *before* task execution.
    *   Caching the assembled image for future use.

**3. Prefetching & Scheduling Integration:**

*   Modify the Container Service to:
    *   Accept Composition Manifests in addition to standard image references.
    *   When a task is scheduled, the Container Service analyzes the Composition Manifest.
    *   Based on the manifest, the Container Service identifies the layers that need to be prefetched onto the compute resource.
    *   The Container Service instructs the Assembly Service to prefetch and assemble the image.
    *   The scheduling algorithm considers both network cost (for layers needing transfer) and CPU cost (for assembly) when selecting the compute resource.

**4.  Dynamic Layer Prioritization:**

*   Implement a layer prioritization algorithm within the Assembly Service.
*   The algorithm prioritizes layers based on:
    *   Size (download larger layers first).
    *   Dependency (layers with more dependent layers are prioritized).
    *   Historical Access Patterns (layers frequently accessed are kept in a faster cache).
    *   Cost (prioritize downloading layers that minimize the overall cost).

**Pseudocode (Container Service – Task Scheduling):**

```
function scheduleTask(task, cluster):
  manifest = task.imageManifest
  if manifest.type == "COMPOSITION":
    layerDAG = manifest.layerDAG
    missingLayers = findMissingLayers(layerDAG, computeResource.cache)
    prefetchLayers(missingLayers, computeResource)
    assembledImage = assembleImage(layerDAG, computeResource) //Assembly Service call
    task.image = assembledImage
  else:
    //Standard image prefetch logic
    prefetchImage(task.image, computeResource)

  //Schedule task with pre-loaded image
  executeTask(task, computeResource)
```

**Benefits:**

*   Reduced Network Transfer: Only missing layers are downloaded.
*   Faster Task Startup: Image assembly happens before task execution.
*   Improved Resource Utilization:  Dynamically build images optimized for the task.
*   Flexibility:  Supports a wider range of image compositions.