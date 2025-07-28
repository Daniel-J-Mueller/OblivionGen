# 10135916

## Dynamic Health Check Weighting Based on User Impact

**Concept:** The current patent focuses on removing unhealthy servers. This expands on that by *dynamically weighting* health checks based on real-time user impact analysis. Instead of simply marking a server 'unhealthy' and removing it, we assess *who* is affected by its degraded performance and adjust health check frequency & weighting accordingly. Critical user segments (identified by attributes like subscription level, geographic location, or specific application usage) receive higher-priority health checks on associated servers.

**System Specifications:**

1.  **User Impact Profiler (UIP):**
    *   Input: Real-time user session data (user ID, location, subscription tier, current application/feature usage).
    *   Processing:  Assigns each user session to one or more ‘Impact Groups’ (e.g., 'Platinum Subscribers - US East', 'Free Tier - EU').  Maintains a dynamic count of active sessions within each Impact Group associated with each server.
    *   Output:  A mapping of servers to Impact Groups, with an associated active session count for each group.  This data is updated every 5 seconds.

2.  **Weighted Health Check Scheduler (WHCS):**
    *   Input:  Data from the UIP, server health status, configurable weighting parameters.
    *   Processing:
        *   Calculates a 'Weighting Score' for each server based on the following:
            *   Impact Group Session Counts: Higher session counts in high-priority Impact Groups increase the weighting score.
            *   Server Load: Current CPU/memory utilization of the server.  Higher load *decreases* the weighting score.
            *   Historical Performance:  Recent error rates and latency data for the server.  Poor historical performance *decreases* the weighting score.
        *   Adjusts the frequency and type of health checks sent to each server based on its Weighting Score.  Servers with high scores receive more frequent, more comprehensive checks (e.g., full transaction simulations).  Servers with low scores receive fewer, simpler checks (e.g., basic ping tests).
    *   Output:  A dynamic schedule of health checks for each server, specifying the check type, frequency, and any associated data.

3.  **Adaptive Thresholding Engine (ATE):**
    *   Input: Health check results, server performance data, ATE Configuration
    *   Processing: ATE is a machine learning model which is re-trained using the new incoming data.
        *   Dynamic determination of acceptable thresholds.
        *   Anomaly detection based on baseline performance of each server and the cluster.
        *   Flags potential issues before they escalate.
    *   Output: Refined thresholds and anomaly flags.

4. **Integration Points:**
    *   Connects to existing DNS resolution systems as described in the patent.
    *   Requires access to user session data (authentication/authorization system).
    *   Requires access to server performance metrics (monitoring system).

**Pseudocode (WHCS):**

```
function calculate_weighting_score(server_id, impact_group_counts, server_load, historical_performance):
  score = 0
  for impact_group, count in impact_group_counts:
    score += count * impact_group_priority[impact_group] #Impact Group Priority is a dictionary
  score -= server_load * server_load_penalty
  score -= historical_performance_penalty * historical_performance
  return score

function adjust_health_check_schedule(server_id, weighting_score):
  if weighting_score > high_threshold:
    health_check_frequency[server_id] = high_frequency
    health_check_type[server_id] = comprehensive_check
  elif weighting_score > medium_threshold:
    health_check_frequency[server_id] = medium_frequency
    health_check_type[server_id] = standard_check
  else:
    health_check_frequency[server_id] = low_frequency
    health_check_type[server_id] = basic_check
```

**Novelty:** This system doesn’t just react to server failure; it *proactively* prioritizes health checks based on user impact. This reduces latency and improves the user experience, especially during peak load or partial outages. It transforms health checking from a purely reactive system to a predictive, user-centric one.