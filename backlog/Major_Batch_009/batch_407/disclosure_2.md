# 9325761

## Dynamic Content 'Blending' via Weighted Provider Contribution

**Concept:** Extend the content provider selection beyond *choosing* a single provider to *dynamically blending* content streams from multiple providers *concurrently*, weighted by the factors described in the patent. This aims to optimize viewing experience – resolution, buffering, even subtle stylistic differences – in real-time.

**Specs:**

*   **Component 1: Content Segmentation Engine:**
    *   Input: Video stream (any format)
    *   Process: Divides the video stream into short, overlapping segments (e.g., 2-5 seconds each). Overlap is crucial for smooth transitions. Each segment is encoded into multiple quality levels (e.g., 360p, 720p, 1080p).
    *   Output: Segment manifest – a list of segments with associated quality levels and timestamps.

*   **Component 2: Provider 'Bidding' System:**
    *   Input: Segment manifest, User data (from patent claims), Weighting profile.
    *   Process: For each segment, the system queries multiple content providers for their ability to deliver that segment at each quality level. Providers ‘bid’ based on current network conditions, server load, and content availability. The ‘bid’ is a cost/performance ratio.
    *   Output: Provider bids per segment and quality level.

*   **Component 3: Dynamic Weighting & Selection:**
    *   Input: Provider bids, User data, Weighting profile.
    *   Process:
        1.  Calculate a weighted score for each provider per segment/quality level. The weighting profile (from patent) determines the importance of factors like cost, quality, and availability.
        2.  Select the provider offering the optimal weighted score for that segment.  The system may prioritize different providers for different segments based on real-time conditions.
        3.  This is done per segment in a rolling fashion, continuously.
    *   Output:  Provider selection list – which provider delivers each segment.

*   **Component 4: Stream Assembly Engine:**
    *   Input: Provider selection list, Video segments from selected providers.
    *   Process: Assembles the final video stream by seamlessly concatenating segments received from different providers. Error correction and smoothing algorithms minimize any visual artifacts due to switching providers.
    *   Output: Final blended video stream for playback.

**Pseudocode (Dynamic Weighting & Selection):**

```
function calculateWeightedScore(providerBid, userCostWeight, userQualityWeight, userAvailabilityWeight):
  score = (providerBid.cost * userCostWeight) + (providerBid.quality * userQualityWeight) + (providerBid.availability * userAvailabilityWeight)
  return score

for each segment in segmentManifest:
  providerBids = getProviderBids(segment)
  highestScore = -1
  selectedProvider = null

  for each providerBid in providerBids:
    score = calculateWeightedScore(providerBid, userCostWeight, userQualityWeight, userAvailabilityWeight)
    if score > highestScore:
      highestScore = score
      selectedProvider = providerBid

  segment.selectedProvider = selectedProvider
  segment.qualityLevel = selectedProvider.qualityLevel
```

**Novelty:** The current patent focuses on *selecting* a provider. This extends it to *concurrently blending* streams, leveraging multiple providers simultaneously to create a dynamically optimized viewing experience.