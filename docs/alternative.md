# Proposal B · Shared Java service
Codex alternative using the same Lab 6 requirements and Java Swing course UI.

A Swing client submits commands over HTTPS to a Java 21 server. The server authenticates the user, authorizes the requested group or direct expense, runs domain services, and owns a shared relational database. Clients cannot write tables directly. A future web or mobile client can call this same API.

| Operation | Server contract |
|---|---|
| Submit expense | Validate membership, fields and split. In one transaction insert the pending expense, shares, optional receipt and in-app notifications. Return an expense ID. |
| Review expense | Authenticate the reviewer, lock the expense, record the decision, and post shares once upon final approval. Return the committed status. |
| Record settlement | Lock the pair and context, recompute the debt, reject excess, and commit payment and posting together. |
| Read summaries | Filter authorized records on the server. Return committed balances and payment history. |

Each write carries a stable request ID scoped to its actor and operation. A timeout after commit can be retried with the same ID. A mismatched payload is rejected. Conflicting decisions return the committed terminal status. The UI refreshes after reconnecting and requires connectivity for financial writes.

**Deployment.** Distribute the Swing client, operate the API behind TLS, keep the database accessible only to the server, and coordinate API versions, migrations and backups. Two users on different machines see the same records. A server outage prevents new reviews until connectivity returns.

**Validation.** Reuse the domain acceptance cases, then add API authorization, duplicate-request and concurrent-decision tests against the shared database. The architecture offers a direct route to shared clients at the cost of more infrastructure for the course demonstration.
