# Architecture reviews and decision
Both proposals were reviewed against the same scenario. A member enters a $10 group expense for three equal participants. The administrator reviews it. A second click or a retry must leave exactly one set of postings.

| Review criterion | A · Embedded database | B · Shared server |
|---|---|---|
| Correct shares and approval | One local transaction and unique expense/share posting keys support $3.34, $3.33 and $3.33. | Same domain rules and uniqueness constraints, with server-side authorization for each command. |
| Retry after interruption | Reopen the database and retry with the same command ID. | Client must distinguish an unknown result after a timeout from a rejected command. Server deduplicates requests. |
| Course deployment | One install and one database. Review by switching accounts on one machine. | Two client installations can share data, but the server, TLS and database must be available. |
| Testing burden | Domain tests, SQLite integration and a Swing walkthrough. | Also requires network failure, API compatibility and cross-client concurrency coverage. |
| Full-product path | Extract application services behind an API before supporting independent devices. | Web and mobile clients can reuse the API. Separate UIs are still required. |

**Review A.** A meets the course workflow if all demo users access one installation. Its strongest feature is a short, auditable path from approval to posting. Its main limitation is that separate installations have isolated balances. Account permissions protect app operations, while the host OS must protect the database file.

**Review B.** B better fits roommates using separate devices. Its main risk for this lab is spending effort on server deployment and timeout recovery before demonstrating expense correctness. A convincing demo would need an actual shared service and a test where a response is lost after approval commits.

**Decision.** The student selected A, the Swing app with a local database. The local transaction boundary supports the approval demonstration with fewer deployment steps. Revisit B when independent devices must share live data. The final documents adopt A throughout.
