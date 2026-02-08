# Technomancy Setup (the Andy way)

## Workflow Diagram

<img width="1024" height="1024" alt="Technomancy Setup" src="https://github.com/user-attachments/assets/a281ab14-ff60-49f0-b18d-9b3b6886a907" />

---

## Canonical Workflow (How This Should Run Every Time)

### 1. Activate the Orchestrator

You start all large or ambiguous workflows by activating the Orchestrator.

You give it a high-level goal, for example:

"Build authentication using Cognito Hosted UI and Google, and integrate it into the existing architecture."

You do not activate the Planner directly for large work.

The Orchestrator is the entry point and owns coordination, not implementation.

---

### 2. Orchestrator Validates Permissions (Manager if needed)

Before any planning or execution:

- The Orchestrator ensures agent permissions exist
- If `AGENT_PERMISSIONS.md` does not exist, the Orchestrator immediately invokes the Manager
- If the requested work introduces new folders, domains, or concerns, the Orchestrator invokes the Manager to update permissions
- The Orchestrator does not proceed until permissions cover the planned work

The Manager is the sole authority allowed to create or modify `AGENT_PERMISSIONS.md`.

No other agent may change permissions.

---

### 3. Orchestrator Invokes the Planner (Always)

The Orchestrator does not design solutions.

It always invokes the Planner first, for every request it handles.

The Planner determines:

- What work needs to be done
- In what order
- Which agents are required
- What can be parallelized vs serialized
- What constraints matter (architecture, UX, security, permissions)

The Planner produces:

- A structured plan
- Ordered phases
- Explicit agent ownership per phase
- Clear deliverables
- Identified permission requirements

The Planner does not execute anything.
The Planner does not propose code.
The Planner does not make architecture or UX decisions.

---

### 4. Orchestrator Executes the Plan (By Delegation Only)

Once a plan exists and permissions are validated, the Orchestrator executes the plan strictly by delegation.

The Orchestrator:

- Routes architecture decisions to Architect
- Routes infrastructure work to Infra
- Routes user experience definition to UX
- Routes business logic implementation to App
- Routes validation to QA and Security

The Orchestrator:

- Enforces execution order
- Ensures agents stay within their role boundaries
- Prevents scope expansion
- Ensures permissions are respected

The Orchestrator does not:

- Write code
- Modify files
- Run commands
- Diagnose bugs
- Make technical judgments

It coordinates. It does not build.

---

### 5. Mandatory Review Gates (Before Completion)

Before work is considered complete, the Orchestrator invokes all review agents.

These are hard gates, not suggestions.

#### QA Gate

QA verifies:

- Correctness
- Regressions
- Sanity
- Alignment with the plan

#### Security Gate

Security verifies:

- Authentication handling
- Trust boundaries
- Data exposure
- Least-privilege assumptions

#### Acceptance Gate (Business Value Check)

Acceptance answers questions like:

- Does this deliver real user or business value?
- Does this feel like a real product or a demo?
- Is the UI modeling user intent, or just calling APIs?
- Would a reasonable user consider this usable?

If any reviewer fails:

- The Orchestrator converts findings into concrete fix tasks
- Routes them back to the responsible execution agent
- Re-runs the relevant review gates after fixes

The Orchestrator cannot override or reinterpret review results.

---

## Why This Split Matters (And Fixes Past Pain)

### Without an Orchestrator

- Planner tries to both plan and execute
- Agents are invoked opportunistically
- Architecture and UX drift over time

Common failure modes include:

- Unconventional delivery patterns
- Demo-style UIs
- "Click a button -> dump JSON"
- Infrastructure decisions leaking upward into UX
- Execution starting before permissions are clear

---

### With Orchestrator -> Planner -> Agents -> Acceptance

- Intent flows top-down
- Permissions are explicit and enforced
- Architecture and UX constraints are owned by the right agents
- Execution remains aligned with the plan
- Business value is validated before completion
- Agents are reusable across projects

This mirrors proven patterns used in:

- Build systems
- CI pipelines
- Production AI agent frameworks
- Human engineering organizations (PM -> EM -> teams)

---

## How to Think About Each Agent

### Orchestrator (ENTRY POINT)

- The agent you talk to
- Owns coordination and execution flow
- Always invokes the Planner
- Ensures permissions exist before work begins
- Routes work to the correct agents
- Enforces review gates
- Never implements or diagnoses

Think: delivery manager, not senior IC.

---

### Planner

- Produces structured plans and phase graphs
- Identifies agent ownership and sequencing
- Identifies permission requirements
- No implementation
- No design decisions
- No fixes

---

### Manager (Permissions Authority)

- Owns `AGENT_PERMISSIONS.md`
- Sole agent allowed to edit permissions
- Applies least-privilege changes
- Invoked automatically when scope or structure changes

---

### Architect

- Defines patterns, boundaries, and constraints
- Steers away from fragile or unconventional designs
- Aligns with common, accepted architectures
- Pushes back on clever-but-wrong ideas

---

### Infra

- Implements infrastructure changes
- Uses common, modern cloud patterns
- Avoids documented infrastructure anti-patterns
- Favors scalable, low-cost defaults
- Produces deployable infrastructure artifacts

---

### UX

- Translates domain goals into real user behavior
- Designs screens, flows, and interactions that feel like a real product
- Rejects demo or API-shaped UIs
- Encourages experience APIs over stateful frontends

---

### App

- Implements business logic
- Assumes architecture and UX decisions are already settled
- Does not invent patterns or workflows

---

### Security

- Validates authentication and authorization handling
- Reviews trust boundaries and data exposure
- Ensures security concerns are addressed in the correct layer

---

### QA

- Verifies correctness, regressions, and sanity
- Confirms outputs match the plan and expectations

---

### Acceptance

- Determines whether the slice delivers real value
- Rejects technically-correct-but-useless output
- Acts as a business-facing quality gate

---

## Rule of Thumb

Small, focused task -> you may invoke a specialist agent directly  
Large or ambiguous work -> always invoke the Orchestrator

If you ever find yourself thinking:

"I'm not sure which agent to start with..."

That is your signal to start with the Orchestrator.
