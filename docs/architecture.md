# Selected architecture · Local Swing application
A single Java 21 process hosts the Swing interface, application services and domain model. A JDBC persistence layer stores data in one embedded SQLite database. Accounts share that database on the same installation. A second device would need a shared service to see the same data.

| Boundary | Responsibilities |
|---|---|
| Presentation | Collect fields, show errors and results, and refresh balances after commit. Run database work off Swing’s event-dispatch thread and update widgets on that thread. |
| Application | Use an authenticated session for every command. Authorize group membership or direct involvement before reading or writing. Coordinate transactions for creation, review and settlement. |
| Domain | Validate amounts and split instructions, determine eligible reviewers, enforce terminal states and calculate exact shares. |
| Storage | Persist users, memberships, expenses, shares, receipts, reviews, notifications, postings and payments. Use foreign keys and unique command/posting keys. Derive balances and reports from committed rows. |

**Deployment.** Package the Java runtime, Swing application and SQLite JDBC driver for the course workstation. On first launch, create a versioned schema in the user's application-data directory. Apply migrations transactionally before opening the UI. Protect the data directory with OS permissions and store passwords as salted adaptive hashes. The trusted workstation owner controls the local files. Use a consistent database backup and check that restoring it reproduces the same balances.

**Failure boundaries.** Obtain a SQLite write transaction before reading mutable state for a review or settlement. On contention, retry the whole command under the same identifier. On failure, roll back the decision and all its postings together. When the result of a commit is uncertain, look up the command ID before retrying. Indexed expense, membership and posting queries support scoped summaries. Deployment and performance remain proposed until implementation.

**Future clients.** Preserve the domain and application boundaries when moving to a centrally hosted service. Replace local session and storage adapters, expose authorized APIs, migrate the data and build platform-specific interfaces. The desktop design supplies a model for that work.
