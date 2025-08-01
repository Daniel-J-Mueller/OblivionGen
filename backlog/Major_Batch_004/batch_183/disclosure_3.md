# 11250191

**Dynamic Coverage Model Synthesis from Formal Verification Results**

**Specification:**

1.  **Core Concept:**  Instead of a manually created or static coverage model, automatically synthesize a coverage model directly from the output of a formal verification engine (e.g., a property checker).

2.  **Components:**
    *   **Formal Verification Engine Interface:**  A module to connect to and extract verification results (proofs, counterexamples, reachable states) from a formal verification tool.  Supports multiple formal engines via a plugin architecture.
    *   **Coverage Point Generator:**  Analyzes the formal verification results to generate coverage points. Specifically:
        *   **Proof Traces -> Coverage Paths:** Extract successful execution paths from proofs as positive coverage targets.
        *   **Counterexample Analysis:** Analyze counterexamples to identify failing conditions.  These failing conditions are translated into negative coverage targets (assert statements that should *not* be triggered).
        *   **Reachability Analysis:** Determine which states/conditions are reachable by the design. This drives coverage for corner cases and state exploration.
    *   **Coverage Model Compiler:** Compiles the generated coverage points into a standard coverage model format (e.g., VCD, custom script for an existing coverage tool).  Supports different abstraction levels – e.g., gate-level, RTL.
    *   **Dynamic Model Update Mechanism:** Allows for the coverage model to be updated *without* re-running simulations.  Changes in the RTL or design spec trigger a re-synthesis of the coverage model based on re-running formal verification.

3.  **Workflow:**
    1.  Formal verification is run on the design.
    2.  The Formal Verification Engine Interface captures the results (proofs, counterexamples).
    3.  The Coverage Point Generator analyzes these results to create a coverage model.
    4.  The Coverage Model Compiler creates a usable coverage model.
    5.  The coverage model is used with the existing stimulus data (from the provided patent) to aggregate coverage.
    6.  If the design changes, steps 1-5 are repeated *without* requiring simulation.

4.  **Pseudocode (Coverage Point Generation):**

    ```
    FUNCTION generate_coverage_points(formal_verification_results)
        coverage_points = []

        // Process Proof Traces
        FOR EACH proof_trace IN formal_verification_results.proof_traces
            coverage_points.append(create_coverage_path(proof_trace))

        // Process Counterexamples
        FOR EACH counterexample IN formal_verification_results.counterexamples
            coverage_points.append(create_negative_coverage_point(counterexample))

        //Process Reachability Information
        FOR EACH reachable_state IN formal_verification_results.reachable_states
            coverage_points.append(create_state_coverage_point(reachable_state))

        RETURN coverage_points
    END FUNCTION

    FUNCTION create_coverage_path(proof_trace):
        //Convert proof trace into a sequence of assertions or conditions to check in coverage model
        //e.g.,  IF (signal_A == 1 AND signal_B == 0) THEN coverage_hit
        RETURN coverage_path
    END FUNCTION

    FUNCTION create_negative_coverage_point(counterexample):
        //Convert counterexample into an assertion that should *not* occur
        //e.g., ASSERT (signal_C != 1 OR signal_D != 0)
        RETURN negative_coverage_point
    END FUNCTION

    FUNCTION create_state_coverage_point(reachable_state):
        //Convert reachable state into a condition that should be met
        //e.g., IF (state_variable == valid_state) THEN coverage_hit
        RETURN state_coverage_point
    END FUNCTION
    ```

5.  **Considerations:**
    *   Scalability: Handling large formal verification outputs.
    *   Abstraction: Choosing the right level of abstraction for the coverage model.
    *   Integration: Seamless integration with existing EDA tools and coverage methodologies.