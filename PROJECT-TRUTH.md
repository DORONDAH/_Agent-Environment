# PROJECT TRUTH

**Project:** AI Agent Orchestrator  
**Project Root:** `C:\Users\doron\Desktop\Labs\_Agent-Environment`  
**Status:** Environment Construction  
**Document Role:** Project Source of Truth  
**Version:** 0.1  
**Authority:** Project Architecture

---

## 1. Purpose

This project establishes an AI Agent Orchestration Environment in VS Code.

The environment is designed to coordinate multiple AI roles with strict separation of:

- Architecture and Decision
- Orchestration and Management
- Execution
- Validation
- Human Escalation

The project is not currently a business/application project.

It is the construction of the agent environment itself.

---

## 2. Relationship to Workspace Standard

This project does **not** replace, modify, or redefine the existing `_Workspace-Standard`.

The workspace standard remains authoritative for workspace-wide:

- agent behavior;
- professional terminology;
- decision processes;
- engineering principles;
- validation;
- documentation;
- safety and approval practices.

This project adds project-specific architecture and current-state truth.

Conceptual hierarchy:

```text
_Labs Workspace
│
├── _Workspace-Standard
│   ├── AI-AGENT-DECISION-FRAMEWORK.md
│   ├── AI-PROFESSIONAL-VOCABULARY.md
│   └── Other Workspace Standards
│
└── _Agent-Environment
    └── PROJECT-TRUTH.md
        ↓
        Project Architecture
        ↓
        Implementation
        ↓
        Validation / Evidence
```

The Project Truth must conform to the Workspace Standard.

It must not silently redefine workspace-wide rules.

---

## 3. Current Workspace Structure

The currently established Labs structure is:

```text
C:\Users\doron\Desktop\Labs
│
├── _Workspace-Standard
├── _Agent-Environment
├── Projects
└── README.md
```

`_Agent-Environment` is currently the selected project root for the AI Agent Orchestrator.

The existing `_Workspace-Standard` must remain unchanged unless a separate, explicit decision is made outside this project.

---

## 4. Current Project State

### Observed / Confirmed

- `_Agent-Environment` is the selected project root.
- The project being constructed is the **AI Agent Orchestrator**.
- The environment is intended to operate inside VS Code.
- Nemotron is connected to native VS Code Chat through a Custom Endpoint.
- Continue is currently used as the Worker interface.
- Continue is connected to OpenRouter.
- Small/free models are intended for simple execution tasks.
- There is currently no direct communication channel between Continue Workers and Nemotron.
- GPT-5.6 is the architectural authority.
- Nemotron is responsible for execution management and supervision.
- Workers perform limited/simple execution.
- Human escalation is required when Nemotron cannot safely continue.
- Autonomous execution is desired within defined Decision Gates.

### Not Yet Implemented / Unknown

The following must not be treated as existing capabilities until verified:

- Orchestrator implementation.
- Orchestrator ↔ Nemotron communication layer.
- Nemotron ↔ Worker communication through the Orchestrator.
- Shared task/state protocol.
- Event or message bus.
- Worker result validation protocol.
- Automatic Worker selection/routing.
- Automatic escalation mechanism.
- Persistent orchestration state.
- Complete logging/audit mechanism.
- Final project directory structure.
- Final model configuration and routing policy.

Unknowns remain unknown until directly verified.

---

# 5. Core Architecture

The intended architecture is:

```text
                         ┌─────────────────────┐
                         │      GPT-5.6        │
                         │      ARCHITECT      │
                         │                     │
                         │ Decision Authority  │
                         └──────────┬──────────┘
                                    │
                           Decisions / Instructions
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │      ORCHESTRATOR   │
                         │                     │
                         │ State / Routing /   │
                         │ Context / Control   │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │      NEMOTRON       │
                         │                     │
                         │ Manager / Supervisor│
                         │ Senior Executor     │
                         └───────┬───────┬─────┘
                                 │       │
                       manages   │       │ executes
                                 │       │
                                 ▼       ▼
                            ┌────────┐  Tools /
                            │Workers │  Environment
                            └────┬───┘
                                 │
                                 ▼
                              Results
                                 │
                                 ▼
                             Nemotron
                                 │
                         Validation / Review
                                 │
                  ┌──────────────┴──────────────┐
                  │                             │
                Success                       Failure
                  │                             │
                  ▼                             ▼
              Continue                    Recovery / Retry
                                                │
                                                ▼
                                      Still unable to proceed
                                                │
                                                ▼
                                           HUMAN GATE
                                                │
                                                ▼
                                               User
```

---

# 6. Authority Model

Authority is deliberately separated from execution.

## GPT-5.6 — Architect

GPT-5.6 is the architectural and decision authority.

Responsibilities:

- Understand objectives.
- Analyze architecture.
- Determine appropriate approach.
- Define constraints.
- Produce decisions.
- Produce instructions for Nemotron.
- Review significant results.
- Determine next architectural direction.
- Preserve architectural integrity.

GPT-5.6 does **not** perform implementation work.

GPT-5.6 communicates operationally through the Orchestrator/Nemotron path and does not directly manage Workers.

Core rule:

```text
GPT-5.6 decides.
Nemotron executes and supervises.
```

---

# 7. Nemotron — Execution Manager

Nemotron is the senior execution and supervision layer.

Responsibilities:

- Receive architectural instructions.
- Translate approved instructions into execution tasks.
- Execute complex tasks directly.
- Manage Workers.
- Assign simple tasks to Workers.
- Inspect Worker results.
- Validate Worker results.
- Detect Worker failures.
- Correct or retry failed Worker execution.
- Take over tasks when a Worker cannot complete them.
- Perform integration and execution validation.
- Report meaningful results back toward the Architect.
- Escalate to the Human when safe continuation is not possible.

Nemotron does not replace the Architect.

Nemotron must not independently redefine project architecture when a decision belongs to the Architect or Human.

---

# 8. Workers

Workers are limited execution agents.

Current Worker interface:

```text
Continue
    ↓
OpenRouter API
    ↓
Small / Free Model
```

Workers are intended for:

- simple;
- bounded;
- low-context;
- well-defined;
- repeatable

tasks.

Workers should not independently perform:

- architecture decisions;
- system-wide reasoning;
- authority decisions;
- major design changes;
- ambiguous tasks;
- high-risk decisions.

Workers operate under Nemotron supervision.

---

# 9. Worker Supervision

A Worker result is not automatically trusted.

The intended lifecycle is:

```text
Nemotron
    ↓
Task Definition
    ↓
Worker
    ↓
Result
    ↓
Nemotron Review
    ↓
Validation
    │
    ├── Valid → Continue
    │
    └── Invalid → Retry / Correct / Take Over
```

The Worker must therefore be treated as an execution component with limited trust.

The system must prefer verification over assumption.

---

# 10. Communication Architecture

The intended future communication path is:

```text
GPT-5.6
    ↓
Orchestrator
    ↓
Nemotron
    ↓
Orchestrator
    ↓
Worker
    ↓
Orchestrator
    ↓
Nemotron
    ↓
Orchestrator
    ↓
GPT-5.6
```

Direct architectural coupling between GPT-5.6 and individual Workers is not desired.

Direct unmanaged coupling between Nemotron and individual Workers is also not the target architecture.

The Orchestrator should eventually provide the controlled communication boundary.

### Current Gap

Continue Workers currently have no established communication channel with Nemotron.

This is an architectural gap to be solved during project construction.

It must not be represented as an already-existing capability.

---

# 11. Decision Gates

The system is intended to be highly autonomous within approved boundaries.

Autonomy does not mean unrestricted authority.

A Decision Gate is required whenever execution reaches a boundary requiring:

- architectural change;
- materially different strategy;
- elevated risk;
- ambiguous requirements;
- security-sensitive action;
- destructive or difficult-to-reverse action;
- unresolved dependency;
- failure outside the approved recovery path;
- human judgment.

The existing Workspace Decision Framework requires explicit approval before controlled implementation and requires a new approval when material information changes the approved approach.

---

# 12. Human Escalation

Human escalation is a mandatory safety mechanism.

When Nemotron cannot safely continue:

```text
STOP
  ↓
Preserve Evidence
  ↓
Classify Failure
  ↓
Determine Available Recovery
  ↓
If Safe and Approved → Continue
  ↓
Otherwise
  ↓
HUMAN ESCALATION
```

The system must never compensate for uncertainty by inventing an architectural decision.

High-consequence situations require stronger verification and stronger human control.

---

# 13. Validation

Successful execution does not automatically mean successful outcome.

Validation must prove the intended result.

The Workspace Standard defines four validation levels:

1. Command Validation
2. State Validation
3. Functional Validation
4. Integration Validation

The highest justified validation level should be used.

For this project, validation should eventually cover:

```text
Component
    ↓
Communication
    ↓
Task Execution
    ↓
Worker Result
    ↓
Nemotron Validation
    ↓
Orchestration State
    ↓
End-to-End Behavior
```

---

# 14. Failure Handling

When execution fails:

1. Stop unnecessary expansion of the blast radius.
2. Preserve evidence.
3. Classify the failure.
4. Attempt only approved recovery.
5. Reassess if the environment differs materially from expectations.
6. Escalate when the approved recovery path is insufficient.
7. Never hide partial failure.

These rules are inherited from the Workspace Decision Framework.

---

# 15. Context Management

Context must be treated as a controlled resource.

GPT-5.6 is a decision/architecture resource and should not be unnecessarily consumed by repetitive execution detail.

The Orchestrator should eventually provide selective context transfer:

```text
Large Environment
      ↓
Relevant State
      ↓
Relevant Task Context
      ↓
Nemotron
      ↓
Relevant Result
      ↓
GPT-5.6
```

The goal is to provide each component only the context required for its responsibility.

---

# 16. Token Efficiency

GPT-5.6 must be used efficiently.

The architecture should avoid unnecessary transmission of:

- full repositories;
- irrelevant logs;
- repetitive Worker output;
- redundant context;
- implementation details that Nemotron can manage.

GPT-5.6 should receive:

- objectives;
- important architectural context;
- decisions;
- relevant evidence;
- exceptions;
- significant results;
- escalation information.

Nemotron should absorb the majority of execution-level context.

---

# 17. Separation of Concerns

The following boundaries are architectural invariants:

```text
Architecture ≠ Execution

Decision ≠ Implementation

Orchestration ≠ Worker

Worker ≠ Authority

Validation ≠ Execution

Context ≠ Authority

GPT-5.6 ≠ Worker

Nemotron ≠ Architect

Human Escalation ≠ Automatic Decision
```

The existing Workspace Vocabulary explicitly defines Separation of Concerns, Single Source of Truth, Decision Gates, Verification Gates, and Escalation as distinct concepts.

---

# 18. Source of Truth

For this project:

```text
PROJECT-TRUTH.md
```

is the authoritative project-level description of:

- project identity;
- project architecture;
- project decisions;
- current state;
- known gaps;
- constraints;
- roles;
- boundaries.

It does not override Workspace Standards.

The general rule remains:

```text
Workspace Standard
        ↓
Project Truth
        ↓
Project Architecture
        ↓
Implementation
        ↓
Validation Evidence
```

---

# 19. Truth Classification

Every important project statement must be classifiable as one of:

### OBSERVED

Directly verified from the environment.

### DECLARED

Defined by project configuration or documentation but not necessarily reverified.

### DECIDED

Explicitly approved architectural/project decision.

### ASSUMED

Currently believed but not verified.

### UNKNOWN

Insufficient evidence.

Unknown information must never be presented as established fact.

This follows the existing Workspace Decision Framework's context model.

---

# 20. Current Architectural Decisions

The following decisions are currently established:

| Decision | Status |
|---|---|
| `_Agent-Environment` is the current project root | DECIDED |
| Project is AI Agent Orchestrator | DECIDED |
| `_Workspace-Standard` remains unchanged | DECIDED |
| `PROJECT-TRUTH.md` is project-level Source of Truth | DECIDED |
| GPT-5.6 is Architect | DECIDED |
| GPT-5.6 does not execute implementation work | DECIDED |
| Nemotron manages execution | DECIDED |
| Nemotron supervises Workers | DECIDED |
| Workers perform simple bounded tasks | DECIDED |
| Continue is current Worker interface | OBSERVED / DECLARED |
| OpenRouter is Worker model API path | DECLARED |
| Worker ↔ Nemotron communication is not yet implemented | OBSERVED |
| Autonomous operation is desired | DECIDED |
| Decision Gates are mandatory control boundaries | DECIDED |
| Human escalation is required when Nemotron cannot safely continue | DECIDED |
| GPT-5.6 usage should be token-efficient | DECIDED |

---

# 21. Current Construction Phase

The project is currently in:

```text
PHASE 0 — ENVIRONMENT CONSTRUCTION
```

The immediate goal is not to build a large application.

The immediate goal is to establish the smallest reliable orchestration foundation that can later support:

```text
Architect
    ↓
Orchestrator
    ↓
Nemotron
    ↓
Workers
    ↓
Tools / Environment
```

Each layer should be constructed and validated before adding unnecessary complexity.

The Workspace Standard favors incremental, observable, minimally invasive, repeatable, reversible execution.

---

# 22. Initial Construction Principle

The project must be built from verified foundations upward.

Preferred construction order:

```text
Verified Environment
        ↓
Project Structure
        ↓
Orchestrator Contract
        ↓
Nemotron Communication
        ↓
Task / Result Contract
        ↓
Worker Communication
        ↓
Worker Supervision
        ↓
Validation
        ↓
Decision Gates
        ↓
Human Escalation
        ↓
End-to-End Orchestration
```

Do not implement higher-level automation before lower-level communication and state boundaries are understood and validated.

---

# 23. Definition of Done

A project component is not considered complete merely because code exists.

Completion requires:

- objective defined;
- relevant context inspected;
- architecture respected;
- implementation approved where required;
- execution completed;
- result validated;
- failures disclosed;
- documentation updated;
- resulting state unambiguous.

This follows the existing Definition of Done.

---

# 24. Non-Goals

At this stage the project is explicitly **not** attempting to:

- replace `_Workspace-Standard`;
- redesign existing Workspace Standard documents;
- make Workers autonomous decision-makers;
- give Workers architectural authority;
- allow Nemotron to redefine architecture independently;
- send all project context to GPT-5.6 by default;
- build unnecessary multi-agent complexity before the basic communication path works.

---

# 25. Architectural Invariant

The central project invariant is:

```text
GPT-5.6
    DECIDES

Orchestrator
    COORDINATES

Nemotron
    MANAGES + EXECUTES + SUPERVISES

Workers
    PERFORM LIMITED TASKS

Human
    RESOLVES ESCALATED DECISIONS
```

Any implementation that violates this separation requires an explicit architectural decision.

---

# 26. Immediate Next Objective

The next objective is to inspect the empty `_Agent-Environment` and design its **minimum viable project structure**.

No implementation should be created until the proposed structure and its responsibilities are understood and approved.

---

**END OF PROJECT TRUTH**