# Technomancy Setup (the Andy way)

## Workflow Diagram

<img width="1024" height="1024" alt="Technomancy Setup" src="https://github.com/user-attachments/assets/a281ab14-ff60-49f0-b18d-9b3b6886a907" />

---

## Canonical Workflow (How This Should Run Every Time)

### 1️⃣ Activate the Orchestrator

You start all large or ambiguous workflows by activating the Orchestrator.

You give it a high-level goal, for example:

“Build authentication using Cognito Hosted UI and Google, and integrate it into the existing architecture.”

You do not activate the Planner directly for large work.

---

### 2️⃣ Orchestrator Validates Permissions (Manager if needed)

Before any planning or execution:

- The Orchestrator checks `AGENT_PERMISSIONS.md`
- If the file does not exist, the Orchestrator immediately invokes the Manager
- If new folders, domains, or concerns are introduced, the Orchestrator invokes the Manager to update permissions

The Manager is the sole authority allowed to create or modify `AGENT_PERMISSIONS.md`.

No other agent may change permissions.

---

### 3️⃣ Orchestrator Invokes the Planner

The Orchestrator’s job at this stage is not to design solutions.

Instead, it asks the Planner to determine:

- What work needs to be done?
- In what order?
- Which agents are required?
- What can be parallelized vs serialized?
- What constraints matter (architecture, UX, security, permissions)?

The Planner produces:

- A structured plan
- Ordered phases
- Explicit agent responsibilities
- Clear deliverables per phase

The Planner does not execute anything.

---

### 4️⃣ Orchestrator Executes the Plan

Once a plan exists, the Orchestrator executes it.

The Orchestrator:

- Calls Architect for architecture decisions
- Calls Infra for CDK / cloud changes
- Calls UX to define real user behavior
- Calls App for application logic
- Calls Security to validate trust boundaries and auth
- Calls QA to sanity-check outcomes

The Orchestrator also:

- Enforces execution order
- Resolves conflicts between agents
- Prevents agents from overreaching their role
- Ensures permissions are respected
- Produces the final integrated output

---

### 5️⃣ Acceptance Gate (Business Value Check)

Before work is considered “done”, the Orchestrator invokes Acceptance.

Acceptance answers questions like:

- Does this deliver real user or business value?
- Does this feel like a real product or a demo?
- Is the UI modeling user intent, or just calling APIs?
- Would a reasonable user consider this usable?

If Acceptance fails, the Orchestrator loops back to the appropriate phase.

---

## Why This Split Matters (And Fixes Past Pain)

### Without an Orchestrator

- Planner tries to both plan and execute
- Agents are invoked opportunistically
- Architecture and UX drift over time

Common failure modes include:

- Unconventional delivery patterns
- Demo-style UIs
- “Click a button → dump JSON”
- Infrastructure decisions leaking upward into UX

---

### With Orchestrator → Planner → Agents → Acceptance

- Intent flows top-down
- Permissions are enforced explicitly
- Architecture and UX constraints are applied early
- Execution remains aligned with the original plan
- Business value is validated before completion
- Agents can be reused across projects

This mirrors proven patterns used in:

- Build systems
- CI pipelines
- Production AI agent frameworks
- Human engineering organizations (PM → EM → teams)

---

## How to Think About Each Agent

### Orchestrator (ENTRY POINT)

- This is the agent you talk to
- Owns execution and coordination
- Enforces permissions and order
- First call is usually the Planner
- Final call is Acceptance

---

### Planner

- Produces plans, phases, and task graphs
- No implementation
- No architecture decisions beyond sequencing

---

### Manager (Permissions Authority)

- Owns `AGENT_PERMISSIONS.md`
- Sole agent allowed to edit permissions
- Defines which agents can touch which areas
- Invoked automatically when scope changes

---

### Infra

- Owns infrastructure implementation
- Implements the architecture decisions from Architect
- Keeps infra modern and standard
- Optimizes for your goals
- Produces deployable CDK changes

---

### Architect

- Decides patterns, boundaries, and constraints
- Steers away from unconventional or fragile designs
- Aligns with common, accepted architectures
- Pushes back on “clever but wrong” designs

---

### UX

- Translates domain goals into real UI behavior
- Produces “this feels like a real app” outcomes
- Rejects demo or API-shaped UIs
- Encourages experience APIs over stateful frontends

---

### App

- Implements business logic
- Assumes architecture and UX decisions are already settled

---

### Security

- Validates authentication, trust boundaries, and data exposure
- Ensures auth is handled in the right layer

---

### QA

- Verifies correctness, regressions, and sanity
- Ensures outputs match the plan

---

### Acceptance

- Determines whether the slice delivers real value
- Rejects technically-correct-but-useless output
- Acts as a business-facing quality gate

---

## Rule of Thumb

Small, focused task → you may invoke a specialist agent directly  
Large or ambiguous work → always invoke the Orchestrator

If you ever find yourself thinking:

“I’m not sure which agent to start with…”

That’s your signal to start with the Orchestrator.
