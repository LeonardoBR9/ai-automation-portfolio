# AI-assisted Operations Workflow

How changes to automation systems are planned, approved, executed and verified when AI assistants are part of the team. The goal is speed **without** giving any single agent unchecked write access to production.

```mermaid
flowchart LR
    R[Request] --> P[1. Planner agent<br/>diagnoses and proposes]
    P --> H{2. Human approval}
    H -- rejected / changes --> P
    H -- approved --> X[3. Executor agent<br/>applies the approved plan]
    X --> V[4. Independent validator<br/>(the planner, in a fresh session)]
    V -- fails --> P
    V -- passes --> D[5. Documented and closed]
```

## Roles

| Step | Actor | Responsibility |
|------|-------|----------------|
| 1. Plan | **Codex** (planner) | Read-only investigation. Produces a diagnosis, the risk, a minimal reversible plan and a handoff with explicit acceptance criteria. Never changes anything. |
| 2. Approve | **Human** | Reads the plan, edits scope, approves or rejects. Anything touching production, data, credentials, integrations or automations needs an explicit "go". |
| 3. Execute | **Claude Code** (executor) | Applies only what the approved handoff says, in small reversible steps, keeping a record of every change and leaving the system in a known state. |
| 4. Validate | **Codex** (validator) | Independently re-checks the result against the acceptance criteria using its own evidence (it does not trust the executor's report). |
| 5. Document | Planner / human | Records what changed, how it was verified and how to roll back. |

## Principles

- **Separation of duties** — the agent that plans is not the agent that executes, and the executor never grades its own work.
- **Read before write** — mapping and diagnosis are always allowed; mutation requires approval.
- **Minimal, reversible, documented** — the smallest change that satisfies the criteria, with a rollback path written down before execution.
- **Intent is not authorization** — an action verb in a request ("fix", "create") means *analyze and plan*; execution is a separate, explicit decision.
- **Fail closed** — if classification, risk or scope is unclear, stop at the plan and ask.
- **Evidence over reports** — validation uses fresh observations (logs, queries, test runs), not the executor's summary.
- **Sanitize on the way out** — anything leaving the private environment (docs, repos, screenshots) is scanned for secrets and identifiers first.

## Change levels

1. **Direct answer** — explanations, reading logs, reviewing a workflow. No ticket.
2. **Technical preparation** — an analysis that may become a change. Produces a short plan; no execution.
3. **Formal task** — any real change to production systems. Written handoff, human approval, execution, independent validation, report.

## Handoff template (abridged)

```
Goal:            one sentence
Scope / limits:  what may and may not be touched
Pre-checks:      state to confirm before starting
Steps:           numbered, minimal, reversible
Rollback:        exact way back
Acceptance:      observable criteria the validator will check
```
