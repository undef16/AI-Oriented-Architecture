# AI Oriented Architecture — Glossary

## AIOA

AI Oriented Architecture. A software architecture framework that adds AI-assisted development as an explicit design constraint. Focuses on minimizing the context an AI agent must gather before safely understanding, modifying, testing, or verifying a change.

See also: [FAQ - What is AI Oriented Architecture?](FAQ.md#what-is-ai-oriented-architecture)

## Crystallization Radius

The amount of system context an AI agent must traverse before it can safely understand, modify, test, or verify a component. Smaller radius means more local reasoning, fewer dependency jumps, and lower modification risk.

This is the primary optimization metric in AIOA. Every architectural decision should answer: how much additional context must an AI agent consume before making a safe change?

See also: [FAQ - What is Crystallization Radius?](FAQ.md#what-is-crystallization-radius)

## Semantic Integrity

The preservation of domain meaning across system boundaries. Ensures each domain term maps to exactly one meaning, preventing AI agents from collapsing similar-looking concepts.

See also: [FAQ - What is Semantic Integrity?](FAQ.md#what-is-semantic-integrity)

## Semantic Collision

The erasure of domain boundaries that occurs when AI agents encounter linguistically similar concepts. The agent collapses distinct domain meanings into a single representation, causing incorrect assumptions.

See also: [FAQ - What is Semantic Collision?](FAQ.md#what-is-semantic-collision)

## Code Crystallization

The process of reducing architectural indirection. Makes behavior explicit, boundaries clear, and reasoning paths local, reducing the overhead AI agents face when traversing abstraction layers.

See also: [FAQ - What is Code Crystallization?](FAQ.md#what-is-code-crystallization)

## Quantum Spectrum

The decomposition boundaries defining component size: Micro (complete feature), Nano (focused capability), and Pico (atomic unit of behavior).

See also: [FAQ - What is the Quantum Spectrum?](FAQ.md#what-is-the-quantum-spectrum)

## Reasoning Territory

The subset of a codebase that an AI agent must understand to perform a task. Smaller reasoning territories reduce context consumption and increase modification safety.

## Agent-Safe Path

A repository path that can be modified by an AI agent without affecting unrelated components. Agent-safe paths have explicit boundaries, local reasoning, and deterministic contracts.

## Context Waste

Tokens consumed by an AI agent that do not contribute to the task at hand. Caused by excessive indirection, hidden dependencies, or scattered business logic.

## TIP (Technical Implementation Pattern)

Specific techniques for reducing Crystallization Radius at different layers of the system. Each TIP addresses a specific architectural concern.

See: [TIP-002](../TIP-002.md) through [TIP-008](../TIP-008.md)
