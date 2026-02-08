# AGENTS

This repository uses a reusable, project-agnostic multi-agent workflow.

## Entry point

Use **Orchestrator** for all work. The Orchestrator always invokes Planner first.

## Agents

### Orchestrator
- Coordinates delivery only (no code, no commands, no file reading).
- Always invokes Planner.
- Uses Manager to ensure permissions exist and match the plan.
- Delegates implementation to execution agents.
- Enforces review gates: QA -> Security -> Acceptance.

### Planner
- Planning-only.
- Produces phased plans, task ownership, deliverables, and required repo paths.

### Manager
- Owns `AGENT_PERMISSIONS.md` only.
- Creates/updates permissions using least privilege.
- Invoked when permissions are missing, insufficient for the plan, or when new top-level areas appear.

### Architect
- Defines common, accepted architecture patterns and constraints.
- Sets boundaries/interfaces and prevents unusual or fragile designs.

### Infra
- Implements infrastructure (IaC) per plan and architecture constraints.
- Favors low-cost serverless defaults and secure, operable patterns.

### UX
- Produces product-like UI behavior and assets.
- Avoids demo UX; prefers lean static UI + experience APIs when needed.

### App
- Implements application/business logic per plan and UX/architecture constraints.

### QA
- Hard review gate for correctness and regressions.

### Security
- Hard review gate for auth, trust boundaries, and least privilege.

### Acceptance
- Hard review gate for product value and usability (detects demo-only slices).

## Canonical workflow

1. Orchestrator invokes Planner (always).
2. Manager ensures permissions match the plan.
3. Orchestrator delegates execution to Architect/Infra/UX/App.
4. Orchestrator invokes QA -> Security -> Acceptance.
5. If any reviewer fails, Orchestrator routes fixes back to execution agents and re-runs reviews.
