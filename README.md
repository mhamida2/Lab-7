# SplitSmart | CIS 285 Lab 7
Architecture comparison and UML modeling · University of Michigan–Dearborn

SplitSmart helps groups and individuals record shared expenses, approve charges, and track who owes whom. This lab designs a Java Swing course prototype. The example used throughout is a group member submitting an expense for administrator review.

| Lab artifact | What to review |
|---|---|
| Starting requirements | [Lab 6 customer request](https://github.com/mhamida2/SplitSmart-Lab6/blob/main/UserRequirement.md), [feature specification](https://github.com/mhamida2/SplitSmart-Lab6/blob/main/specs/001-splitsmart/spec.md), and [constitution](https://github.com/mhamida2/SplitSmart-Lab6/blob/main/.specify/memory/constitution.md) |
| Two proposals | [A · Spec Kit baseline](specs/001-splitsmart/plan.md) and [B · Codex alternative](docs/alternative.md) |
| Analysis and choice | [Review reports and student decision](docs/review.md) |
| Consolidation | [Instructions and execution record](docs/consolidation.md) |
| Final documents | [Requirements and acceptance cases](docs/requirements.md), [architecture and deployment](docs/architecture.md), [detailed design](docs/design.md) |
| UML | [Class diagram](diagrams/classes.svg) and [activity diagram](diagrams/approval.svg), with editable [class](diagrams/classes.puml) and [activity](diagrams/approval.puml) sources |

The selected architecture runs Swing and the expense service in one process with an embedded database. Multiple demo users take turns signing in on one installation. The deliverables are design documents and diagrams.
