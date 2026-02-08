# Technomancy Agents

This repository uses a reusable, project-agnostic multi-agent system.

## Entry point

For any large or ambiguous task, activate **Orchestrator**.

Orchestrator will:
1. Ensure permissions exist (via **Manager** and `AGENT_PERMISSIONS.md`)
2. Invoke **Planner** to produce a phased plan
3. Execute the plan by delegating to specialist agents
4. Enforce architecture + UX guardrails early
5. Invoke **Acceptance**, **Security**, and **QA** before declaring completion

## Permissions

Permissions are not embedded in agent YAML files.

They live in a repo-root file:

- `AGENT_PERMISSIONS.md`

Orchestrator and Planner must read and obey this file before work begins.
If it doesn't exist, Orchestrator must invoke Manager to create it.

## Agents

### Orchestrator
- Executes work end-to-end by invoking other agents
- Enforces order, resolves conflicts, produces integrated output

### Planner
- Planning-only
- Produces phased plans, task graphs, deliverables, validation steps

### Architect
- Defines boundaries, constraints, and standard patterns
- Prevents architectural drift

### UX
- Produces real, modern UI behavior that feels like a real app
- Drives screen flows and recommends Experience APIs (BFF) to keep the UI lean

### Infra
- Implements AWS/CDK changes following common, production-grade patterns
- Defaults to managed/serverless services, least-privilege IAM, and low idle cost unless the plan says otherwise

### App
- Implements application logic and API/Experience API shapes
- Keeps business logic server-side

### Security
- Validates auth, trust boundaries, least privilege, data exposure

### QA
- Automated + manual validation, regression checks, deploy readiness

### Acceptance
- Product acceptance testing
- Flags "demo" outcomes and ensures the slice provides real user value

### Manager
- Owns permissions only
- May edit **only** `AGENT_PERMISSIONS.md`
