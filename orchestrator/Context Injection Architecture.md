# CONTEXT INJECTION ARCHITECTURE

**Project:** AI Agent Environment  
**Status:** Approved Architecture  
**Version:** 1.0

---

## 1. Purpose

This document defines how the AI Agent Environment automatically assembles and injects the correct context into each Agent execution.

Agents must not be required to manually locate, open, or interpret their operating contracts before every task.

The Orchestrator is responsible for assembling the required runtime context.

Core principle:

> **The Agent receives the context it needs. The Agent does not manage its own context loading.**

---

## 2. Architecture

```text
GPT-5.6
Architect / Decision Authority
        │
        ▼
Orchestrator
State / Routing / Context Assembly / Control Boundary
        │
        ▼
Nemotron
Execution Manager / Supervisor
        │
        ▼
Orchestrator
        │
        ▼
Worker
Bounded Execution Agent
```

No direct architectural coupling is created between GPT-5.6 and Workers.

No direct control coupling is created between Nemotron and Workers outside the Orchestrator boundary.

---

## 3. Context Layers

Every Agent execution receives context in layers.

### Layer 1 — Global Context

Applies to the environment as a whole.

Examples:

```text
PROJECT-TRUTH.md
Workspace Standards
AI-PROFESSIONAL-VOCABULARY.md
AI-AGENT-DECISION-FRAMEWORK.md
```

Only the relevant portions should be injected when full context is unnecessary.

---

### Layer 2 — Role Context

Defines how the receiving Agent operates.

For Nemotron:

```text
contracts/NEMOTRON-OPERATING-CONTRACT.md
```

For Workers:

```text
contracts/WORKER-OPERATING-CONTRACT.md
```

The role contract is automatically included by the Orchestrator according to the Agent role.

The Agent is not expected to manually load its own contract.

---

### Layer 3 — Task Context

Defines the current execution request.

Task context may contain:

```text
task_id
objective
scope
inputs
constraints
allowed_actions
allowed_tools
expected_output
validation_requirements
stop_conditions
escalation_conditions
```

Task context is specific to the current execution.

---

### Layer 4 — Relevant Runtime Context

The Orchestrator may inject additional context required for the specific task.

Examples:

```text
relevant files
current state
previous execution results
worker results
validation evidence
errors
dependencies
environment information
```

Only relevant information should be included.

---

## 4. Context Assembly

Before invoking an Agent, the Orchestrator performs Context Assembly.

Conceptually:

```text
Agent Context =
    Global Context
  + Role Context
  + Task Context
  + Relevant Runtime Context
```

The Orchestrator must not blindly inject the entire environment.

Context should be:

- relevant
- sufficient
- current
- traceable
- minimal where possible

---

## 5. GPT-5.6 → Nemotron

GPT-5.6 provides the architectural decision.

The Orchestrator converts that decision into an execution context for Nemotron.

Conceptually:

```text
Architect Instruction
        +
Global Context
        +
Nemotron Contract
        +
Relevant Runtime Context
        ↓
Nemotron Runtime Context
```

The resulting instruction must preserve the architectural intent of GPT-5.6.

Nemotron may determine **how** to execute within the approved boundaries.

Nemotron must not reinterpret the architecture or silently expand the approved scope.

---

## 6. Nemotron → Worker

Nemotron defines a bounded execution task.

The Orchestrator assembles the Worker context.

Conceptually:

```text
Bounded Worker Task
        +
Relevant Project Context
        +
Worker Contract
        +
Required Inputs
        ↓
Worker Runtime Context
```

Workers receive only the context required to perform their bounded task.

Workers do not receive unnecessary architectural context unless it is required for execution.

---

## 7. Worker → Nemotron

Workers return structured execution results through the Orchestrator.

Minimum result:

```text
task_id
status
actions_performed
output
validation
evidence
errors
remaining_work
```

The Orchestrator correlates the result with the originating task.

Nemotron receives the result for review and validation.

Worker output is considered **untrusted until reviewed and validated by Nemotron**.

---

## 8. Nemotron → GPT-5.6

After execution and validation, Nemotron reports the relevant state back through the Orchestrator.

The report should contain:

```text
instruction_id
execution_status
work_completed
validation_status
evidence
errors
remaining_work
scope_exceptions
escalation_required
```

GPT-5.6 receives the information required to make the next architectural decision.

---

## 9. Context Precedence

When multiple context sources exist, authority follows this order:

```text
Human Authority
        ↓
GPT-5.6 Architectural Decisions
        ↓
PROJECT-TRUTH
        ↓
Workspace Standards
        ↓
Role Operating Contract
        ↓
Approved Task Scope
        ↓
Runtime State
        ↓
Agent Suggestions
        ↓
Worker Output
```

Lower-level information cannot silently override higher-level authority.

Observed runtime state may reveal that an assumption or instruction is no longer valid. In such cases the Agent must stop, reconcile state, and escalate when required.

---

## 10. Context Classification

Runtime information must be distinguishable as:

```text
OBSERVED
DECLARED
ASSUMED
UNKNOWN
```

Unknown information must never be presented as established fact.

The Orchestrator should preserve this distinction when assembling context.

---

## 11. Context Efficiency

The system must optimize context usage.

The Orchestrator should prefer:

```text
Relevant Context
```

over:

```text
Entire Environment
```

For example, a Worker modifying one configuration file should not automatically receive unrelated project documentation.

Context should be assembled using:

1. Role
2. Task
3. Dependencies
4. Required files/state
5. Validation requirements
6. Relevant prior results

This reduces token consumption and decreases irrelevant information.

---

## 12. Contract Loading Rule

Operating contracts are **runtime configuration for Agent behavior**, not instructions that Agents must manually retrieve.

Incorrect model:

```text
Start Agent
    ↓
Tell Agent to read contract
    ↓
Agent searches filesystem
    ↓
Agent interprets contract
    ↓
Agent begins work
```

Required model:

```text
Start Task
    ↓
Orchestrator identifies Agent role
    ↓
Orchestrator loads required contract
    ↓
Orchestrator assembles context
    ↓
Agent starts with required context
```

---

## 13. No Manual Context Management

Normal execution must not require the user to manually provide:

- operating contracts
- project truth
- role definitions
- previous execution state
- standard reporting formats
- routine validation requirements

These are responsibilities of the Agent Environment and Orchestrator.

Human interaction should be reserved primarily for:

- architectural decisions
- approvals required by Decision Gates
- unresolved ambiguity
- material scope changes
- unsafe or blocked execution
- escalation decisions

---

## 14. State and Correlation

Every execution must be traceable.

The system should maintain identifiers such as:

```text
instruction_id
task_id
execution_id
parent_id
```

These identifiers allow the Orchestrator to correlate:

```text
GPT Decision
    ↓
Nemotron Instruction
    ↓
Worker Task
    ↓
Worker Result
    ↓
Nemotron Validation
    ↓
Final Report
```

The exact identifier implementation is an implementation concern and is not prescribed by this document.

---

## 15. Context Freshness

The Orchestrator must prefer current state over stale state.

Before executing against mutable resources, relevant state should be reconciled.

If the environment differs materially from the context originally assembled:

```text
STOP
↓
Reconcile State
↓
Determine Impact
↓
Continue only if still within approved scope
OR
Escalate
```

The system must not continue execution based on known-invalid context.

---

## 16. Failure Handling

If required context cannot be assembled:

```text
Do not start execution.
```

The Orchestrator should report:

```text
missing_context
source
impact
required_action
```

If context is contradictory:

```text
Do not silently choose one source.
```

The conflict must be resolved according to authority and escalation rules.

If context is stale:

```text
Refresh / Reconcile
```

before continuing.

---

## 17. Security and Scope

Context injection must follow least-privilege principles.

An Agent should receive only:

```text
the authority
the information
the tools
the scope
```

required for its current role and task.

Context injection must not become a mechanism for granting an Agent additional authority.

Receiving information does not automatically grant permission to modify it.

---

## 18. Current Implementation State

At the time of this document:

### Implemented

```text
PROJECT-TRUTH.md
contracts/NEMOTRON-OPERATING-CONTRACT.md
contracts/WORKER-OPERATING-CONTRACT.md
agents/nemotron/
agents/workers/
orchestrator/
```

### Not Yet Implemented

The runtime Context Injection mechanism itself is not yet implemented.

The Worker ↔ Nemotron execution interface is also not yet implemented.

The existing Worker interface is currently based on Continue/OpenRouter.

These are implementation tasks that follow this architecture.

---

## 19. Definition of Done

Context Injection Architecture is considered correctly implemented when:

- Agents receive their required contracts automatically.
- Agents do not need to manually search for their contracts during normal execution.
- Nemotron automatically receives the required global, role, task, and runtime context.
- Workers automatically receive the Worker Contract and bounded task context.
- Context is minimized to relevant information.
- Context remains traceable to its originating instruction/task.
- Stale or contradictory context is detected.
- Missing required context prevents unsafe execution.
- Worker results can be correlated back to their originating task.
- Nemotron can supervise execution without manually reconstructing context.
- GPT-5.6 receives the execution state required for the next architectural decision.
- Human intervention is required only where the defined Decision Gates or escalation conditions require it.

---

## 20. Core Rule

> **The Orchestrator owns Context Assembly.  
> Agents own execution within the context they receive.  
> Contracts define behavior.  
> Tasks define scope.  
> Runtime state defines what is actually happening.**