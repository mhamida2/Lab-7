# Spec Kit baseline plan · SplitSmart
Feature 001-splitsmart · 6 November 2026 · [Input specification](https://github.com/mhamida2/SplitSmart-Lab6/blob/main/specs/001-splitsmart/spec.md)

## Summary
Build a layered desktop application. A member records a group expense in Swing. An application service checks membership and validates the split, then saves a pending expense, its shares and its notifications. When the administrator approves it, the same local database transaction records the decision and posts the shares. Every screen reads the same committed financial history.

## Technical context
Java 21, Maven, Swing and JDBC with embedded SQLite. Use BigDecimal for domain money and exact integer cents in storage. JUnit 5 is the target for future tests. A single machine hosts multiple named accounts for the demo. No central server is needed. Store optional receipt bytes in the database so saving a receipt and its expense is atomic. No throughput target was specified.

## Constitution check
The service and domain layers own access checks and financial rules. Swing handles input and display. The design preserves web and mobile as full-product goals. PlantUML defines the models, and acceptance cases trace back to Lab 6 identifiers. Human review precedes acceptance.

## Proposed project structure
| Java package | Responsibility |
|---|---|
| ui | Swing forms, field errors, pending approvals, balances and reports |
| application | Session-aware commands for membership, expense submission, review and settlement |
| domain | Expense, approval, payment, split calculation and balance rules |
| persistence | JDBC repositories and transaction boundaries for a single database |

## Delivery and validation
First exercise split and status rules independently. Next exercise transactions with a disposable SQLite database, including rollback and duplicate approval. Finally run a two-account desktop walkthrough. Package one desktop application and keep its writable database in the OS user’s application-data directory. Back up with SQLite's consistent backup facility. A second installation has its own data. Multi-device sharing requires a different deployment boundary.
