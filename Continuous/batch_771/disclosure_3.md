# 9865254

## Dynamic FST Pruning via Reinforcement Learning

**Concept:** Instead of static FST compression/decompression, utilize a Reinforcement Learning (RL) agent to dynamically prune and reconstruct portions of the FST *during* runtime, adapting to the specific audio input. This allows for a finer-grained approach than pre-compression, potentially yielding significant resource savings without impacting accuracy.

**Specs:**

*   **Agent:** A Deep Q-Network (DQN) or Policy Gradient agent.
*   **State:** The agent's state is a vector representing:
    *   Current acoustic feature vector window.
    *   Activation levels of key nodes in the FST (representing probability mass).
    *   Remaining computational budget (time/energy).
*   **Actions:** The agent can take the following actions on FST arcs/nodes:
    *   *Prune:* Remove an arc/node from the active FST subgraph.
    *   *Restore:* Re-add a previously pruned arc/node.
    *   *Hold:*  Maintain the current state of an arc/node.
    *   *Expand:* Instantiate a previously compressed arc or node.
*   **Reward:** The reward function is a weighted combination of:
    *   ASR accuracy (primary driver).
    *   Computational cost (penalty for excessive computation).
    *   Latency (penalty for delayed output).
*   **Training:** The RL agent is trained offline using a large corpus of audio data and corresponding transcriptions.  Curriculum learning can be employed, starting with simpler audio examples and gradually increasing complexity.
*   **FST Representation:** Maintain a compressed and uncompressed representation of the FST in memory.  Pruning and restoration operations are fast due to the pre-compressed state.
*   **Runtime Operation:**
    1.  Receive audio feature vector.
    2.  The RL agent observes the current state.
    3.  The agent selects an action.
    4.  The FST is updated based on the selected action.
    5.  ASR processing is performed on the modified FST.
    6.  The reward is calculated.
    7.  The agent's policy is updated (online learning can further refine the policy during runtime).

**Pseudocode (Runtime):**

```
function process_audio(audio_features):
    state = get_current_state(audio_features)
    action = agent.select_action(state)
    modified_fst = apply_action(action, current_fst)
    output = perform_asr(audio_features, modified_fst)
    reward = calculate_reward(output, computational_cost)
    agent.update_policy(state, action, reward)
    return output
```

**Innovation:**

This approach differs from static FST compression in its *adaptability*. It moves from a "one-size-fits-all" compressed FST to a dynamically adjusted FST tailored to each audio input. This is achieved through intelligent pruning and restoration guided by the RL agent.  The system learns which parts of the FST are critical for different acoustic conditions and optimizes resource usage accordingly.