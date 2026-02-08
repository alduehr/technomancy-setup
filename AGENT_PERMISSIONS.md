# AGENT_PERMISSIONS.md

This file defines which agents may modify which parts of the repository.
It is intentionally simple and uses least-privilege defaults.

## Rules

- Orchestrator and Planner must read this file before doing any work.
- If this file is missing, Orchestrator must invoke Manager to create it.
- If new top-level folders or major subsystems are introduced, Orchestrator must
  invoke Manager to update permissions.

## Permission table

| Agent | May modify | Notes |
|---|---|---|
| orchestrator | docs/** | Orchestrator coordinates work; avoids direct code edits when specialists exist |
| planner | (none) | Planning-only; should not modify repo files |
| architect | docs/** | Architecture notes/ADRs only, unless explicitly granted more |
| ux | ui/**, docs/** | UI behavior/specs and static UI assets |
| infra | cdk/**, infra/**, docs/** | CDK/infrastructure code and docs |
| app | src/**, packages/**, docs/** | Application code, APIs, shared libs |
| security | docs/** | Security notes/checklists; does not modify code unless granted |
| qa | tests/**, docs/** | Tests and docs |
| acceptance | docs/** | Acceptance criteria and release gates |
| manager | AGENT_PERMISSIONS.md | Manager may edit only this file |

## Notes

- Adjust paths to match your repo structure.
- Keep permissions as narrow as possible.
