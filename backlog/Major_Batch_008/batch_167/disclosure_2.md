# 11082808

## Adaptive Multicast Frame Fragmentation & Reassembly

**Concept:** Extend the unbuffered multicast delivery mode by introducing dynamic fragmentation of multicast frames based on receiver capabilities and network congestion. This allows for a more robust and efficient delivery even in scenarios with limited bandwidth or heterogeneous receiver power-saving modes.

**Specifications:**

**1. Receiver Capability Advertisement:**

*   Each receiver (computing device) periodically broadcasts a “Capability Advertisement” frame alongside standard beacon requests.
*   The Capability Advertisement includes:
    *   Maximum Supported Frame Size (MSFS): The largest frame size the device can reliably process in a single DTIM interval.
    *   Power Saving Mode Level: An indication of the receiver's current power-saving aggressiveness (e.g., 1-5, 1 being least aggressive, 5 being most).
    *   Buffer Capacity: Estimated buffer space available for temporary storage.

**2. Network Access Device (Wireless Access Point) - Frame Analysis & Fragmentation:**

*   The Wireless Access Point maintains a "Receiver Capability Table" mapping each device MAC address to its advertised capabilities.
*   Upon receiving a multicast frame:
    *   Determine the lowest MSFS amongst all active receivers associated with the multicast group (identified via IGMP).
    *   If the incoming multicast frame exceeds the lowest MSFS:
        *   Fragment the frame into smaller segments, each no larger than the lowest MSFS.
        *   Add a "Fragment Header" to each segment:
            *   Total Segments: Total number of fragments.
            *   Segment Number: Current segment’s sequence number.
            *   Multicast MAC Address: The original multicast MAC address.
        *   Transmit segments within the current DTIM interval.
    *   If the frame is smaller than or equal to the lowest MSFS, transmit as-is.

**3. Receiver - Frame Reassembly:**

*   Upon receiving a fragment:
    *   Check if the fragment’s Multicast MAC Address matches a subscribed multicast group.
    *   Buffer the fragment.
    *   Track received fragments via a reassembly table, keyed by Multicast MAC Address.
    *   When all fragments for a given Multicast MAC Address are received (based on “Total Segments” and “Segment Number”):
        *   Reassemble the fragments into the original frame.
        *   Deliver the reassembled frame to the application layer.
    *   Implement a timeout mechanism to discard incomplete fragments after a predetermined period.

**4. Congestion Aware Fragmentation:**

*   Monitor DTIM interval utilization.
*   If utilization exceeds a threshold:
    *   Dynamically reduce the maximum fragment size for subsequent multicast frames. This prioritizes delivering smaller, more frequent updates, even at the cost of increased overhead.
*   Adjust fragmentation dynamically based on observed network conditions during the DTIM interval.

**Pseudocode (Wireless Access Point - Frame Handling):**

```
function handleMulticastFrame(multicastFrame):
  groupMembers = getGroupMembers(multicastFrame.destinationMAC)
  lowestMSFS = findLowestMSFS(groupMembers)

  if multicastFrame.size > lowestMSFS:
    segments = fragmentFrame(multicastFrame, lowestMSFS)
    for segment in segments:
      transmitSegment(segment, currentDTIMInterval)
  else:
    transmitFrame(multicastFrame, currentDTIMInterval)

function fragmentFrame(frame, maxSegmentSize):
  segments = []
  segmentNumber = 0
  totalSegments = calculateTotalSegments(frame.size, maxSegmentSize)

  while frame.size > 0:
    segmentSize = min(maxSegmentSize, frame.size)
    segment = createSegment(frame.data[:segmentSize], segmentNumber, totalSegments, frame.destinationMAC)
    segments.append(segment)
    frame.data = frame.data[segmentSize:]
    segmentNumber++

  return segments
```

**Potential Benefits:**

*   Increased robustness in heterogeneous networks.
*   Improved efficiency in congested environments.
*   Optimized power consumption for devices in power-saving mode.
*   Adaptability to varying network conditions.