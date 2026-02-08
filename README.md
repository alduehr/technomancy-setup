# Technomancy Setup (the Andy way)

## Workflow Diagram

<img width="1024" height="1024" alt="Technomancy Setup" src="https://github.com/user-attachments/assets/a281ab14-ff60-49f0-b18d-9b3b6886a907" />

## Canonical Workflow (How This Should Run Every Time)

### 1️⃣ Activate the Orchestrator

You start large workflows by activating the **Orchestrator**.

You give it a high-level goal, for example:

> “Build authentication using Cognito Hosted UI and Google, and integrate it into the existing architecture.”

You **do not** activate the Planner directly for large or ambiguous work.

---

### 2️⃣ Orchestrator Invokes the Planner

The Orchestrator’s job at this stage is **not** to think deeply about the solution, but to determine how to approach the work.

The Orchestrator asks the Planner to answer questions such as:

- What work needs to be done?
- In what order?
- Which agents are required?
- What can be parallelized vs. serialized?
- What constraints matter (architecture, UX, security)?

The Planner produces:

- A structured plan
- Ordered phases
- Explicit agent responsibilities
- Clear deliverables per phase

The Planner **does not execute** anything.

---

### 3️⃣ Orchestrator Executes the Plan

Once a plan exists, the Orchestrator is responsible for execution.

The Orchestrator:

- Calls **Architect** for architecture decisions
- Calls **Infra** for CDK / AWS changes
- Calls **UX** to shape UI behavior
- Calls **App** for application logic
- Calls **Security** to validate authentication, boundaries, and risk
- Calls **QA** to sanity-check outcomes

The Orchestrator also:

- Enforces execution order
- Resolves conflicts between agents
- Prevents agents from overreaching their role
- Produces the final integrated output

---

## Why This Split Matters (And Fixes Past Pain)

### Without an Orchestrator

- Planner tries to both plan *and* execute
- Agents are invoked opportunistically
- Architecture and UX drift over time

Common failure modes include:

- UI behind APIs
- Demo-style UIs
- Infrastructure decisions leaking upward into UX

---

### With Orchestrator → Planner → Agents

- Intent flows top-down
- Architecture and UX constraints are applied early
- Execution remains aligned with the original plan
- Agents can be swapped or reused across projects

This mirrors proven patterns used in:

- Build systems
- CI pipelines
- Production AI agent frameworks
- Human engineering organizations (PM → EM → teams)

---

## How to Think About Each Agent

### 🧠 Orchestrator (ENTRY POINT)

- This is the agent you talk to
- Owns execution
- Decides *who* does work and *when*
- First call is usually the Planner

---

### 🗺 Planner

- Produces plans, phases, and task graphs
- No implementation
- No architecture decisions beyond sequencing

---

### 🏗 Architect

- Decides patterns, boundaries, and constraints
- Steers away from anti-patterns without naming them
- Aligns decisions with common, accepted architectures

---

### 🎨 UX

- Translates domain goals into real UI behavior
- Produces “this feels like a real app” outcomes
- Encourages experience APIs over stateful frontends

---

### 🧩 App

- Implements business logic
- Assumes architecture and UX decisions are already settled

---

### 🛡 Security

- Validates authentication, trust boundaries, and data exposure

---

### 🧪 QA

- Verifies correctness, regressions, and overall sanity

---

## Rule of Thumb

- **Small, focused task** → you may invoke a specialist agent directly  
- **Large or ambiguous work** → always invoke the **Orchestrator**

If you ever find yourself thinking:

> “I’m not sure which agent to start with…”

That’s your signal to start with the **Orchestrator**.
