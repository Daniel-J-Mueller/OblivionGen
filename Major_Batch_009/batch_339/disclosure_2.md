# 9367856

**Loyalty Account ‘Shadowing’ & Predictive Expiration**

**Specification:**

**Core Concept:** Extend the running order value system to create ‘shadow’ accounts that mirror the primary loyalty account *before* any transaction is committed. This allows for predictive expiration modeling and proactive incentive adjustments, preventing unexpected loyalty point loss for the user.

**Components:**

*   **Shadow Account Generator (SAG):** Upon receiving *any* update or expiration request for a loyalty account, the SAG instantaneously creates a shadow account. The shadow account mirrors the primary account's balance and sub-balances *at that moment*. It utilizes the running order value as a timestamp for this snapshot.
*   **Transaction Simulator (TS):** All update and expiration requests are *first* applied to the shadow account via the TS. The TS tracks changes in the shadow account, simulating the outcome of the transaction.
*   **Expiration Prediction Engine (EPE):** The EPE analyzes the simulated shadow account state to predict the impact of the transaction on subaccount expiration.  It assesses if applying the transaction would cause a subaccount to fall below an expiration threshold *before* the primary account is updated.
*   **Incentive Adjustment Module (IAM):** If the EPE predicts an undesirable expiration, the IAM proactively adjusts incentives. This could involve temporarily increasing loyalty point accrual rates, offering bonus points, or applying a temporary shield to prevent expiration. Adjustments are calculated *before* applying the transaction to the primary account.
*   **Dual Write Mechanism:** After the transaction is successfully applied to the primary account, the shadow account is purged. This ensures the system maintains a current snapshot only when a transaction is pending.

**Pseudocode (Simplified):**

```
function processRequest(request):
  createShadowAccount(request.accountId)
  shadowAccount = getShadowAccount(request.accountId)

  simulateTransaction(request, shadowAccount)
  predictedState = getShadowAccountState(shadowAccount)

  if predictExpiration(predictedState) == TRUE:
    adjustment = calculateIncentiveAdjustment(predictedState)
    applyAdjustment(adjustment, request)

  applyTransactionToPrimaryAccount(request)
  purgeShadowAccount(request.accountId)
```

**Data Structures:**

*   **Shadow Account:** Mirror of the primary account; includes all subaccount balances and relevant metadata (timestamp = running order value). Stored in a fast-access, volatile cache.
*   **Expiration Rules:** Define expiration thresholds and adjustment strategies based on subaccount type and user profile.
*   **Adjustment History:** Logs all incentive adjustments for audit and performance analysis.

**Novelty:**

This approach differs from the original patent by *proactively* modeling transaction impact before committing changes. The existing patent focuses on prioritizing requests *after* a conflict arises.  This system aims to prevent the conflict from happening in the first place, enhancing user experience and loyalty.  It leverages the running order value system not just for concurrency control, but for creating a predictive simulation environment.