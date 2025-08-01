# 10013974

## Dynamic FST Pruning via Reinforcement Learning

**Concept:** Implement a reinforcement learning (RL) agent to dynamically prune FST arcs during ASR decoding. Unlike static pruning which happens pre-runtime, this system will *learn* which arcs are detrimental to performance *during* decoding, and remove them on the fly. This optimizes the FST for the *specific* audio input, going beyond general compaction.

**Specs:**

*   **FST Representation:** Utilize the existing FST representation (states, arcs, weights) as outlined in the patent. However, add a “prune_flag” to each arc. Initially, all arcs have `prune_flag = False`.
*   **RL Agent:**
    *   **State:** The state observed by the RL agent consists of:
        *   Current FST state ID.
        *   Acoustic score of the current arc.
        *   Language model score of the current arc.
        *   Number of active arcs originating from the current state.
        *   A 'complexity penalty' – a value representing how 'dense' the FST is around the current state (calculated as the standard deviation of arc weights).
    *   **Action:** The agent has two possible actions for each arc:
        *   `KEEP`:  The arc remains active (`prune_flag = False`).
        *   `PRUNE`: The arc is temporarily deactivated (`prune_flag = True`).
    *   **Reward:** The reward function is critical. It’s a combination of:
        *   **ASR Accuracy:**  Positive reward for correct word predictions.
        *   **Decoding Speed:** Positive reward for faster decoding times (calculated as the number of FST states traversed per unit of time).
        *   **Pruning Penalty:** A small negative reward for each pruned arc (to prevent excessive pruning that might harm accuracy). This penalty should be tunable.
*   **Decoding Process:**
    *   During Viterbi decoding, before traversing an arc, the RL agent observes the current state and arc scores.
    *   The agent predicts an action (`KEEP` or `PRUNE`).
    *   If `PRUNE` is selected, the arc is skipped during Viterbi search. The Viterbi algorithm simply won’t consider that path.
    *   After decoding, the agent receives a reward based on the decoding performance.
    *   The agent updates its Q-table (or uses a more sophisticated reinforcement learning algorithm like Policy Gradients) based on the reward and observed state-action pair.
*   **Training:** The RL agent is trained offline using a large corpus of audio data and transcriptions. This allows the agent to learn a pruning strategy that optimizes ASR performance.
*   **Implementation Notes:**
    *   A priority replay buffer should be used to prioritize learning from ‘difficult’ or ‘surprising’ state-action pairs.
    *   Exploration-exploitation trade-off must be managed during training (e.g., using epsilon-greedy or softmax action selection).
    *   The RL agent's parameters (learning rate, discount factor, exploration rate) should be tuned using validation data.



**Pseudocode:**

```
// During Decoding:
For each state in FST:
    For each outgoing arc:
        State = (current_state_id, acoustic_score, language_score, num_active_arcs, complexity_penalty)
        Action = RL_Agent.choose_action(State)  // Returns KEEP or PRUNE
        If Action == PRUNE:
            Skip this arc during Viterbi search
        Else:
            Process arc as usual in Viterbi

// After Decoding:
Reward = calculate_asr_accuracy_reward + calculate_decoding_speed_reward - pruning_penalty * num_pruned_arcs
RL_Agent.update_q_table(State, Action, Reward)

```

This approach creates a highly adaptive ASR system that dynamically optimizes the FST for each input audio stream, leading to improved accuracy and decoding speed.