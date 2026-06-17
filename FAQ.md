# AI Oriented Architecture — Frequently Asked Questions

## What is AI Oriented Architecture?

AI Oriented Architecture (AIOA) is a software architecture framework that adds AI-assisted development as an explicit design constraint. It focuses on reducing the amount of context an AI agent must gather before safely understanding, modifying, testing, or verifying a change.

AIOA does not replace human-oriented architecture. It adds a new optimization target: minimizing context consumption for AI agents.

## What is Crystallization Radius?

Crystallization Radius is the amount of system context an AI agent must traverse before it can safely understand, modify, test, or verify a component. A smaller radius means more local reasoning, fewer dependency jumps, and lower modification risk.

Large Crystallization Radius causes:
- broad repository searches
- multi-service dependency tracking
- excessive abstraction traversal
- context-window inflation
- expensive debugging cycles

Small Crystallization Radius enables:
- localized reasoning
- isolated modifications
- deterministic testing
- predictable retrieval
- lower token consumption

## What is Semantic Integrity?

Semantic Integrity means keeping domain concepts explicit, distinguishable, and stable across the system. It prevents AI agents from collapsing similar-looking concepts into the same meaning.

When multiple domain concepts are linguistically similar, AI agents treat them as interchangeable. This causes Semantic Collision — the erasure of domain boundaries.

## What is Semantic Collision?

Semantic Collision is the erasure of domain boundaries that occurs when AI agents encounter linguistically similar concepts. The agent collapses distinct domain meanings into a single representation, causing incorrect assumptions and broken logic.

Example: a system with Trading Day Time, Tuning Day Time, and Eval Day Time may confuse an agent into treating them as the same concept.

## What is Code Crystallization?

Code Crystallization is the process of reducing architectural indirection. Traditional architecture favors abstraction layers, pass-through components, and delegation chains. These patterns help humans manage complexity.

For AI agents, these same patterns increase reasoning overhead. Every indirection layer forces the agent to traverse additional context before reaching the actual business logic.

Code Crystallization reduces this indirection by making behavior explicit, boundaries clear, and reasoning paths local.

## What is Declarative Straight-Line Code?

Declarative Straight-Line Code is a pattern for reducing control-flow Crystallization Radius. AI agents struggle when business logic is hidden inside procedural control flow: nested conditions, loops, retries, locks, transactions, exception tunnels, callback chains.

Declarative Straight-Line Code replaces procedural mechanics with explicit policies. The business intent becomes linear and explicit. The agent spends context on business meaning, not execution mechanics.

## What is the Quantum Spectrum?

The Quantum Spectrum defines component size boundaries: Micro, Nano, and Pico.

- Micro: complete feature with local state and clear boundaries
- Nano: focused capability within a Micro component
- Pico: atomic unit of behavior

Uncontrolled component growth increases Crystallization Radius. The Quantum Spectrum provides decomposition boundaries to keep reasoning territories small.

## When should I use AIOA?

AIOA is recommended when:
- AI coding agents modify your codebase
- Context window consumption is a bottleneck
- Agent-generated code has high error rates
- Codebase navigation takes more time than code generation
- Multiple teams maintain shared repositories

AIOA is less relevant when:
- No AI agents touch the code
- Codebase is small enough for full-context reasoning
- Changes are infrequent

## How does AIOA relate to microservices?

AIOA is orthogonal to deployment architecture. Whether your system runs as a monolith, microservices, or serverless functions is secondary.

A poorly structured microservice ecosystem may have a larger Crystallization Radius than a well-designed monolith. Conversely, a distributed platform can have a small Crystallization Radius if boundaries, contracts, and reasoning paths are local and explicit.

AIOA evaluates how software is understood and modified, not where it is deployed.

## How do I measure Crystallization Radius?

Measure the context an agent consumes to complete a task:
- Files read before writing a patch
- Tokens consumed during repository navigation
- Time spent on architectural planning vs. code generation
- Error rate on first-attempt modifications
- Number of false starts (agent reads wrong files)

Lower values indicate smaller Crystallization Radius.

## What are the TIPs?

TIPs (Technical Implementation Patterns) are specific techniques for reducing Crystallization Radius at different layers of the system:

- [TIP-002: Semantic Collision](../TIP-002.md) — Prevent domain boundary erasure
- [TIP-003: Repository Search Optimization](../TIP-003.md) — Reduce traversal costs
- [TIP-004: Code Crystallization](../TIP-004.md) — Reduce architectural indirection
- [TIP-005: The Quantum Spectrum](../TIP-005.md) — Control component size
- [TIP-006: Declarative Straight-Line Code](../TIP-006.md) — Reduce control-flow complexity
- [TIP-007: Strict JSON Gateways](../TIP-007.md) — Move uncertainty to boundaries
- [TIP-008: Event-Driven Integration](../TIP-008.md) — Reduce cross-boundary reasoning

## Case Study: 92% AI Search Space Reduction

A tuning subsystem was measured before and after applying AIOA principles.

**Before:** 2333 nodes in the architectural community. The tuning domain was buried inside a large amount of unrelated context.

**After:** 181 nodes. The same tuning workflow rebuilt with AIOA principles: explicit boundaries, isolated responsibilities, local reasoning, event-driven interactions, bounded contexts.

**Result:** approximately 92% reduction in measured AI search space for this subsystem.

This demonstrates that codebase design can be a primary constraint on AI agent performance, not only model capability.
