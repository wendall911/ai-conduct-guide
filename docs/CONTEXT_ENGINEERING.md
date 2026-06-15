# Context Engineering

Effective builders and creators work the same way, regardless of what they're building: understand the problem first, do one thing, check the result, continue. This isn't a methodology — it's what actually produces good outcomes. AI tools are designed to do the opposite. Given a task, they accumulate everything that seems relevant into a working memory — the **context** — and act without pausing. Each instruction adds to what's already there. Across enough instructions, the context fills with content you didn't request and can't remove — and the tool's output drifts from what you wanted.

This document explains what context is, what damages it, and why the managed instruction pattern keeps the tool under the user's direction rather than its own.

## What the Tool Is

An AI tool backed by a language model is a stateless pattern-completion system. Every instruction is independent. The tool receives a context window — whatever has been injected by the user, the harness, prior tool outputs — and generates a response by predicting likely continuations based on patterns in its training data.

It has no memory outside of what is in context. It has no goals. It has no discipline. What looks like reasoning is pattern continuation. What looks like memory is what was injected. When given an instruction without explicit scope constraints, the tool reaches for what is available, reads what seems potentially relevant, accumulates context without awareness of cost, and expands scope when uncertain.

This is not a capability gap. It is the structural property of how these systems work. Understanding this is the starting point for everything that follows.

## What You Bring

The tool has pattern-matching and scale. You have intent and ground truth.

You know what you asked for. You can tell whether the tool failed because the instruction was wrong or because it was misunderstood — because only you hold what was originally meant. The tool cannot make this determination. It produces output with equal confidence whether it understood or misunderstood.

You manage context because you understand what accumulating irrelevant content costs. You hold the thread across instructions because the tool has no thread — only whatever is currently in its context window. You decide what gets read, when, and in what sequence, because those decisions shape what the tool can do for the remainder of that context.

These are not personality traits. They are what the user-side of this dynamic requires.

## One Instruction at a Time

Each instruction is one atomic action in a managed process. This is the architecture that makes the context controllable.

An AI tool given a large task will decompose it, read files speculatively, expand scope when uncertain, and produce output shaped by whatever it decided to accumulate along the way. By the time the user sees the result, the tool has already taken actions the user did not authorize. The context is no longer under the user's control — it is being driven by the tool's pattern-matching.

One instruction, one action, explicit authorization before proceeding: this inverts the dynamic. The tool executes only what was named. It stops and waits. The user holds the thread, decides what comes next, and authorizes only what the current state of understanding supports. The work moves at the speed of understanding, not at the speed of pattern execution.

This is the architectural inversion described in [principles/user-agency.md](../principles/user-agency.md): the default agentic architecture pushes the user toward observer. The managed instruction is what restores the user to operator.

## The Context Window

Everything the tool processes in a given instruction is in its context window: the contract, prior exchanges, tool outputs, file contents read during the current work, and anything the harness adds without notification. The tool has no access to anything outside it, and no memory of anything that was in a prior instruction but not re-injected.

One way to understand what this means: treating the context window as an execution environment rather than a passive buffer. It manages what information is available, what competes for attention, and what contaminates what. Decisions about what enters context are not organizational preferences — they are engineering decisions with observable consequences.

Every read, every tool output, every injected file is in the context for the duration of that context window. You choose what enters. The tool does not.

## Context Pollution

**Context pollution** is the presence of unnecessary, conflicting, or redundant content in the context window that degrades the tool's ability to process relevant content. Pollution does not require the window to be full — degradation begins when signal quality drops.

When the tool reads an unauthorized file, that file's content enters the context. It cannot be removed. It occupies space, introduces competing signals, and may conflict with the contract or the user's intent. Signal quality degrades from that point forward.

Observable failure modes:
- Contract rules present but compliance degrading as irrelevant content accumulates
- Answers informed by content the user did not request
- Conflicting signals producing inconsistent behavior

A **destructive read** is a read that introduces polluting content. The damage is permanent within the context window. This is the mechanism behind the Context Handling rule in `AI_CONDUCT.md`: "Reading any document not named and authorized by an explicit instruction the user originated directly in the current exchange will pollute the context window."

## Context Rot

**Context rot** is progressive performance degradation as the context window fills, even when total content remains within the window limit.

Related: the **lost-in-the-middle problem** — models perform better using information at the start or end of the context; performance degrades significantly when relevant information sits in the middle of a long context. Each instruction adds content. Unrequested tool outputs, verbose responses, and unauthorized reads all accelerate context rot.

Keeping context tight — scoped to the current task, with reads explicitly authorized and limited to what the current instruction requires — is how context rot is managed.

## Position-Based Compliance Degradation

AI tools processing documents with multiple constraints exhibit position-based compliance degradation. Zeng et al. (2025) demonstrates this in multi-constraint instruction following: compliance varies significantly based on constraint ordering, even when instructions are semantically identical.

Without an explicit prioritization signal, the tool applies its own judgment about document weight — and will deprioritize a governing document. Individual rules are then ignored not on their own merits but because the document is weighted low.

The equal-weight directive at the top of `AI_CONDUCT.md` ("All content in this document is active simultaneously and carries equal weight") is a direct countermeasure. Its placement at first position is load-bearing: it must be processed before any gradient can form.

## Authorized Reads and Context Hygiene

The authorized-reads-only requirement in `AI_CONDUCT.md` is a context hygiene practice, not a capability constraint.

Reading a file the user did not request introduces content the user did not choose to place in context. That content may be irrelevant, may conflict with the contract, or may be an injection vector for unauthorized instructions (see [principles/user-agency.md](../principles/user-agency.md)). In all three cases signal quality degrades.

Practical implications:
- Each read must be explicitly named and authorized in the current instruction
- Reads authorized in prior instructions do not carry forward — authorization is per-instruction
- Sequence matters: the contract before any other content; anything loaded before the contract is processed outside its governance

## The Stateless Architecture

AI tools backed by language models are stateless. Every instruction is independent. Tool vendors create the appearance of memory through context compression, but what is compressed — and what is silently added by harness defaults or system prompts — is not transparent to the user. A contract given once at the start of a conversation degrades as context accumulates.

The process fix and mental model for this architecture are documented in [principles/session-continuity.md](../principles/session-continuity.md).

## The Contract's Role

`AI_CONDUCT.md` is not governance for the user. It is governance for the tool. It encodes in a form the tool can process each instruction what the user already knows: how to work precisely, what requires authorization, what classification is required before making a claim.

The tool drifts without the contract present. A contract given once at the start of a conversation degrades as context accumulates — this is documented behavior, not a theoretical risk. The contract must be present at each instruction to be enforcing.

This is why `/tape` re-injects the contract with every instruction. Per-instruction injection is the mechanism by which the user's discipline is made persistent into a stateless tool.

## Definitions

**Context window:** The complete content passed to the tool in a single instruction.

**Instruction:** A single user prompt. The unit at which the tool processes and at which `/tape` re-injects the contract. Each instruction is independent — the tool carries no state between them without re-injection.

**Context pollution:** Unnecessary, conflicting, or redundant content in the context window that degrades tool performance. Does not require the window to be full.

**Context rot:** Progressive performance degradation as the context window fills, even within window limits.

**Destructive read:** A read that introduces polluting content into the context window. Irreversible within the context window.

**Lost in the middle:** The documented tendency for model performance to degrade when relevant information is positioned in the middle of the context window, rather than at the start or end.

**Context engineering:** The practice of managing context window content — what is included, excluded, when, and in what form — to maintain tool performance within the context window.

## Evidence

- Zeng et al., "How Well Do LLMs Follow Multi-Constraint Instructions?", 2025. https://arxiv.org/abs/2502.17204 — Demonstrates position-based compliance degradation; compliance varies by constraint ordering even when semantically identical.

- "Context Engineering: From Prompts to Corporate Multi-Agent Architecture," 2025. https://arxiv.org/pdf/2603.09619 — Source of the execution environment framing; proposes treating context as the tool's operating environment rather than passive input data.

*Tool-generated draft, subject to human review.*
