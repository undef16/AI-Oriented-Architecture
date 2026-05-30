# AI Oriented Architecture Principles

Status: Canonical.

## Purpose

This document defines the canonical principles of AI Oriented Architecture (AIOA).

AIOA is a software architecture approach optimized for systems increasingly developed, maintained, debugged, reviewed, and evolved by AI coding agents.

These principles are intended for:

* human architects;
* AI coding agents;
* code review workflows;
* architecture review gates;
* `spec-kit-aioa` presets;
* automated compliance checks.

AIOA does not replace traditional software engineering.

It adds a new architectural concern:

> How much context must an AI agent traverse before it can safely understand, modify, test, or verify a system?

---

## Core Objective

The core objective of AIOA is:

> Enable safe local reasoning for AI-assisted software development.

A system is more AI-oriented when a change can be understood and performed within a small, explicit, deterministic reasoning boundary.

A system is less AI-oriented when every change requires broad repository traversal, hidden dependency reconstruction, cross-service reasoning, runtime-state guessing, or semantic interpretation across unclear boundaries.

---

## Core Constraints

AIOA is governed by two fundamental constraints.

### C1. Minimize Crystallization Radius

Crystallization Radius is the amount of system context an AI agent must traverse before it can safely understand, modify, test, or verify a component.

A large Crystallization Radius means the agent must inspect many files, layers, services, handlers, logs, events, runtime traces, or historical decisions before making a safe change.

A small Crystallization Radius means the relevant context is local, explicit, and bounded.

### C2. Preserve Semantic Integrity

Semantic Integrity is the property that domain concepts remain explicit, distinct, and protected from semantic collapse.

A system can have a small Crystallization Radius and still fail if distinct business meanings are collapsed into generic or ambiguous structures.

Reducing context must never erase domain meaning.

---

# Canonical Principles

## P1. Prefer Local Reasoning

A feature should be understandable, modifiable, testable, and reviewable inside the smallest practical reasoning boundary.

AI agents should not need global system knowledge for local changes.

### Good

```text
The feature is owned by a clearly named actor.
The relevant contract, behavior, and tests are located nearby.
The agent can modify the behavior without traversing unrelated modules.
```

### Bad

```text
The feature is scattered across controller, mapper, validator, service, factory, mediator, handler, and shared utility layers.
The agent must reconstruct the workflow by following many indirect calls.
```

### Review questions

```text
Can the agent understand this change locally?
What is the smallest boundary that contains the required context?
Does the implementation require global architectural knowledge?
Can any dependency traversal be removed?
```

---

## P2. Minimize Crystallization Radius

Every architectural decision should reduce or preserve the amount of context required for safe change.

AIOA treats context traversal as an architectural cost.

### Good

```text
A change to discount calculation requires reading one actor, one DTO contract, and one local test file.
```

### Bad

```text
A change to discount calculation requires reading pricing service, checkout service, subscription service, shared validators, mapper layers, configuration factories, and integration tests across several unrelated areas.
```

### Review questions

```text
Does this change increase the amount of context an agent must traverse?
Which files or components are required to understand the change?
Can the reasoning path be shortened?
Did this implementation introduce new hidden dependencies?
```

---

## P3. Preserve Semantic Integrity

Distinct domain concepts must remain explicitly named and structurally separated.

AI agents are prone to collapsing linguistically similar concepts into generic identifiers or shared structures.

### Good

```text
TradingTime
TuningTime
EvaluationTime
```

Each concept has a distinct name, role, and contract.

### Bad

```text
process_time
day_time
time_value
```

Different business meanings are collapsed into one generic representation.

### Review questions

```text
Are there similar domain terms that could be confused?
Did the implementation collapse distinct concepts into one generic name?
Should a primitive value become a named value object?
Are business concepts still distinguishable after the change?
```

---

## P4. Make Boundaries Explicit

Architectural boundaries must be visible in code, naming, contracts, and file organization.

A hidden boundary is not a boundary for an AI agent.

### Good

```text
CheckoutActor
PaymentRequested
PaymentCompleted
CheckoutRequestDto
CheckoutResultDto
```

The actor, input contract, output contract, and events are explicit.

### Bad

```text
Processor
Manager
Helper
Data
Payload
Context
```

The boundary and ownership must be inferred from implementation details.

### Review questions

```text
Where is the boundary of this feature?
What enters the boundary?
What leaves the boundary?
Which actor owns the behavior?
Are DTOs, events, and commands explicitly named?
```

---

## P5. Make Contracts Deterministic

DTOs, events, commands, and schemas must provide stable assumptions for AI agents.

Loose inputs create defensive boilerplate and increase reasoning ambiguity.

### Good

```text
Loose JSON is validated at the gateway.
The business layer receives a deterministic DTO.
Required fields are non-nullable.
Enums and value constraints are explicit.
```

### Bad

```text
Business logic receives raw JSON, dictionaries, maps, nullable blobs, or loosely typed payloads.
Core methods repeatedly check for nulls, missing fields, malformed enums, and fallback defaults.
```

### Review questions

```text
Is the input contract explicit?
Is malformed input rejected at the boundary?
Does business logic receive validated DTOs or events?
Are optional and nullable states intentional?
Did technical validation leak into core business logic?
```

---

## P6. Prefer Declarative Straight-Line Execution

Business logic should express intent directly.

Recurring procedural mechanics should be moved into stable primitives, language features, libraries, or prepared infrastructure constructs.

This applies to excessive manual use of:

```text
if / else
for / while
try / catch
lock
manual retry loops
manual transaction scopes
manual resource handling
callback chains
lifecycle plumbing
```

### Good

```pseudo
valid_orders = orders.filter(is_valid)

valid_orders
    .process_with(retry_policy)
    .inside(transaction_boundary)
    .save_successful()
```

The business flow is visible and execution policies are explicit.

### Bad

```pseudo
try {
    lock resource {
        for order in orders {
            if order is valid {
                with transaction {
                    result = process(order)

                    if result failed {
                        retry result
                    }

                    if result succeeded {
                        save result
                    }
                }
            }
        }
    }
}
```

The business intent is buried inside procedural machinery.

### Review questions

```text
Is business logic hidden inside procedural control flow?
Can loops, retries, transactions, locks, or resource handling be expressed declaratively?
Can prepared primitives replace repeated execution scaffolding?
Is the business intent visible without reconstructing procedural mechanics?
```

---

## P7. Decompose by Reasoning Boundaries, Not Deployment Topology

AIOA is orthogonal to deployment architecture.

The same principles apply to:

```text
modular applications
microservice systems
distributed systems
event-driven platforms
hybrid enterprise systems
```

A component may run in one process or across many nodes.

The primary question is not where it runs.

The primary question is how much context is required to reason about it.

### Good

```text
A module, actor, or service owns a clear business responsibility and can be modified independently.
```

### Bad

```text
A system is split into many deployable units, but every change still requires understanding several tightly coupled services.
```

### Review questions

```text
Is this boundary improving local reasoning?
Or is it only creating more deployment and integration surface?
Is the split architectural, cognitive, and semantic?
Or merely infrastructural?
```

---

## P8. Extract Only Under Proven Reuse Pressure

AIOA rejects premature abstraction.

New behavior should start inside the owning business boundary and be extracted only when reuse or isolation pressure becomes real.

### Good

```text
A feature is implemented inside its owning Micro Actor.
A reusable business workflow is later extracted into a Nano Actor.
A deterministic technical primitive is later extracted into a Pico Actor.
```

### Bad

```text
The agent creates interfaces, factories, helpers, shared utilities, and abstract services before any real reuse exists.
```

### Review questions

```text
Is this abstraction justified by real reuse?
Does the extraction reduce reasoning effort?
Did the change create generic utility sprawl?
Is the new component easier to reason about than the original local code?
```

---

## P9. Prefer Event Boundaries for Cross-Component Communication

Components should communicate through explicit events when doing so preserves local reasoning and does not violate business requirements.

Event boundaries reduce direct knowledge of another component’s implementation.

### Good

```text
CheckoutActor publishes PaymentRequested.
PaymentActor handles PaymentRequested and publishes PaymentCompleted.
OrderActor handles PaymentCompleted.
```

Each actor owns a local reaction.

### Bad

```text
CheckoutActor synchronously calls PaymentService, waits for FraudService, checks InventoryService, and then updates NotificationService.
```

The workflow becomes a chain of cross-boundary reasoning dependencies.

### Review questions

```text
Does this direct call expand the reasoning scope?
Would an event boundary make responsibilities clearer?
Are events immutable and business-oriented?
Do handlers remain unaware of each other's internal implementation?
Is synchronous communication required by consistency or latency constraints?
```

---

## P10. Make Runtime State Explainable

Important runtime state should expose enough provenance for debugging and AI-assisted analysis.

An AI agent should not need to reconstruct every state transition from scattered logs, repository search, and stack traces.

### Good

```text
A stateful workflow object records significant mutation slices:
who changed what, when, and from which operation.
```

### Bad

```text
A DTO is mutated across mappers, services, handlers, and background jobs, but the final object contains no explanation of how it reached its state.
```

### Review questions

```text
If this object reaches a bad state, can the agent understand how it happened?
Are important state transitions observable?
Should an ADTO or mutation history be introduced?
Are mapper-driven or PATCH-driven mutations explainable?
```

---

# Required Review Gates

Every AIOA-aware spec, plan, task list, and code review should include the following gates.

## Gate 1. Crystallization Radius Check

```text
Did this change increas
e or reduce the amount of context required for safe modification?
```

## Gate 2. Semantic Integrity Check

```text
Did this change preserve distinct domain meanings?
```

## Gate 3. Local Reasoning Check

```text
Can the change be understood and reviewed inside a small explicit boundary?
```

## Gate 4. Boundary Explicitness Check

```text
Are ownership, inputs, outputs, commands, events, and DTOs explicit?
```

## Gate 5. Contract Determinism Check

```text
Are inputs validated at the boundary and converted into deterministic contracts?
```

## Gate 6. Control-Flow Simplicity Check

```text
Is business intent visible without reconstructing excessive procedural machinery?
```

## Gate 7. Decomposition Quality Check

```text
Are abstractions justified by proven reuse or isolation pressure?
```

## Gate 8. Integration Locality Check

```text
Does communication preserve local reasoning and avoid unnecessary cross-boundary dependency tracking?
```

## Gate 9. Runtime Explainability Check

```text
Can important runtime state transitions be understood without broad forensic reconstruction?
```

---

# Non-Goals

AIOA does not require:

```text
rewriting every system
splitting everything into microservices
eliminating all synchronous communication
removing all abstractions
removing all control flow
replacing human architecture judgment
```

AIOA requires:

```text
making reasoning boundaries explicit
minimizing unnecessary context traversal
preserving domain meaning
making contracts deterministic
making important state transitions explainable
```

---

# Summary

AI Oriented Architecture is governed by two constraints:

```text
Minimize Crystallization Radius.
Preserve Semantic Integrity.
```

Its primary operational goal is:

```text
Prefer Local Reasoning.
```

Every pattern in AIOA exists to make software easier for AI agents and humans to understand, modify, test, debug, and evolve safely.
