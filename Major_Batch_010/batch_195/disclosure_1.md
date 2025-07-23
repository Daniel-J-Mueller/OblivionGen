# 10621576

## Dynamic Payment Token Splitting & Redistribution

**Concept:** Expand the single-use payment token concept to allow a user to *split* a token's value across multiple recipients, or redistribute unused value after a partial redemption.

**Specifications:**

**1. Token Creation & Value Assignment:**

*   **Input:** User specifies total token value, initial recipient(s) with assigned values, and a ‘redistribution enabled’ flag.
*   **Process:** System generates a unique payment token associated with the total value.  The token’s metadata includes a list of initial recipients and their allocated amounts. The ‘redistribution enabled’ flag is stored with the token.
*   **Output:** Payment token transmitted to user.

**2. Partial Redemption:**

*   **Input:** Recipient attempts redemption for an amount less than their allocated value.
*   **Process:**
    *   System verifies token validity.
    *   Redemption amount is deducted from the recipient's allocated value within the token’s metadata.
    *   Remaining allocated value for the recipient is updated.
    *   If 'redistribution enabled', the *unused* portion of the redeemed amount becomes available for reassignment by the token originator.
*   **Output:** Redemption successful/failed notification.

**3. Redistribution Mechanism:**

*   **Input:** Token originator requests redistribution of unused funds.
*   **Process:**
    *   System presents a UI to the originator listing available unused funds and options:
        *   Add new recipient.
        *   Increase allocation to existing recipient.
        *   Return funds to originator’s account.
    *   Originator makes allocation choices.
    *   Token metadata is updated to reflect new allocations.
*   **Output:** Updated token metadata.

**4. System Components:**

*   **Token Management Service:**  Responsible for token creation, storage, and tracking of allocated values.
*   **Redistribution Service:** Handles the reallocation of unused funds based on originator requests.
*   **User Interface:** Web/mobile application for token management and redistribution.
*   **API:** Secure API for communication between services and the UI.

**Pseudocode (Redistribution Service):**

```
function redistributeFunds(tokenId, newAllocations):
  token = getToken(tokenId)
  if token is null:
    return "Token not found"

  unusedFunds = calculateUnusedFunds(token)
  if unusedFunds == 0:
    return "No unused funds available"

  totalAllocated = 0
  for allocation in newAllocations:
    totalAllocated += allocation.amount

  if totalAllocated > unusedFunds:
    return "Total allocation exceeds available funds"

  // Update token metadata with new allocations
  updateTokenAllocations(token, newAllocations)

  return "Funds redistributed successfully"
```

**Potential Use Cases:**

*   **Gift Cards with Flexible Spending:**  A user gives a gift card, but the recipient can choose how to split the value between multiple stores or individuals.
*   **Shared Expenses:** Group of friends can contribute to a single token, then split the value across various bills or purchases.
*   **Dynamic Rewards Programs:** Rewards points can be split among different redemption options or shared with family members.