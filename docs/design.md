# Detailed design
[Class diagram](../diagrams/classes.svg) · [Group approval activity](../diagrams/approval.svg)

## Ownership and identity
An expense has one creator, one payer, one or more share rows, and either one group or a direct context. Membership records retain history. The expense snapshots participant IDs and shares at submission. For direct expenses, store the distinct required reviewers from the involved users excluding the creator. For group expenses, enforce the group administrator rule at decision time. A payer's own share creates no debt to themselves.

## Submission and splitting
Authorize the actor before persisting input. Represent amounts as BigDecimal, require at most two decimal places, and validate positive value, date, description and participants. For equal shares use integer quotient and remainder on total cents and participant count. For percentages multiply exact decimal percentages by total cents and divide by 100. Floor each quota to whole cents, rank the discarded remainders and distribute remaining cents in descending order, then ascending user ID. Store the final allocation and verify the sum. Persist the pending expense, shares, optional receipt and deduplicated notifications together.

## Review and posting
A unique key on (expense, reviewer) admits one decision per reviewer. Rejection requires a reason. Terminal states cannot transition again. A direct expense stays pending until all required approvals exist. On final approval, insert one obligation per nonzero, non-self share from participant to payer, with a unique (expense, participant) source key. Commit these rows with the approved state. Rejecting an expense commits the review and rejected state with zero postings. Repeat submission of the same command returns its committed result. Deny a conflicting terminal decision.

## Settlement and reporting
Within one write transaction, authorize the actor, check the command ID, then compute the current net debt in the specified group or direct context. Record the payment and one offsetting posting only if it is positive, valid and within the debt. A balance is a derived view, calculated by netting postings in both directions for the pair. An account can see its permitted history and group data; summary queries enforce the same access checks as detail views.

## Walkthrough for future verification
1. Sign in as member 2. Record $10 paid by member 2, split equally among members 1, 2 and 3. Members 1 and 3 receive notifications. All balances still read zero in a fresh group.
2. Sign in as the administrator and approve. Member 1 owes member 2 $3.34 and member 3 owes member 2 $3.33. Reopening or repeating the review produces the same result.
3. With a fresh expense, reject and supply a reason. Inspect its history and verify balances are unchanged. Attempt a review as an ordinary member and verify access is denied.
4. For a direct expense with two required reviewers, approve as the first and reject as the second. Verify rejected status and zero postings. Inject a failure during a separate approval transaction and verify rollback before retry.
