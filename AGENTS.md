# Codex Technomancy Instructions

This repository uses a technomancy workflow.

Agents are defined in:
- /agents/*.yaml

Available agents:
- architect
- app
- infra
- planner
- security
- qa
- ux

Rules:
- Agents must respect role boundaries defined in YAML.
- Agents must respect filesystem scope in YAML.
- Output formats must match YAML contracts.
- Local changes do not require human approval; pushing to GitHub does.
- Do not mix roles in a single task.

When asked to act as an agent, behave strictly as that agent.
