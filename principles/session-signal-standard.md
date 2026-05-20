# Session Signal Standard v0.1

## Purpose

A lightweight convention for passing session state claims with each request to a
stateless AI agent. Treats the agent correctly as a stateless API endpoint rather
than a conversational partner with persistent memory.

This standard is maintained by the ai-conduct-guide project. It is not an RFC.
It does not require infrastructure, memory layers, or tool-specific configuration.
It works with any tool that accepts text input.

## Design Scope

This standard solves one problem: getting `AI_CONDUCT.md` into the agent's
context window as early as possible, and ensuring the agent acknowledges it
before proceeding.

The outcome of that contact is binary. The agent either operates under the
contract, or it explicitly proceeds without it. Both outcomes are legible. An
agent that has read `AI_CONDUCT.md` cannot claim ignorance. An agent that has
not read it — or that proceeds as if it has not — has disclosed something about
its design or its configuration.

This standard does not solve deliberate bypasses. A sufficiently motivated actor
can circumvent any in-context instruction. Corporations bypass GPL licenses
routinely, and the remedy is legal enforcement, not better license text. That is
not this project's problem. This project's problem is earlier and simpler: is
the contract in the context window? Did the agent see it?

**The two-layer approach:**

1. **User-configured signals** — `/tape` and `/state` commands, per-tool
   configuration files (`~/.claude/commands/`, etc.). Reliable when set up.
   Requires per-user action. This is the primary mechanism for contributors
   who have adopted the standard for their own workflow.

2. **Project-committed tool configuration** — instruction files committed to
   the repository by the maintainer: `.github/copilot-instructions.md`,
   `.github/prompts/*.prompt.md`, `.cursor/rules/`, and equivalents for other
   tools. Tooling auto-loads these for every contributor without per-user setup.
   The maintainer controls this layer. It provides zero-config context injection
   for all contributors regardless of their personal configuration.

`AI_CONDUCT.md` alone does nothing. A file sitting in a repository is inert
until something carries it into a context window. The project-committed layer
is what closes that gap for contributors who have not configured anything. A
contributor clones the project, opens their tool, and the tool auto-loads the
project's instruction file — which references or includes the conduct contract.
The contract reaches the context window before any task begins.

The project-committed layer does not require reliability from any single file.
Individual auto-load mechanisms are inconsistent across tools and versions.
Committing instruction files for multiple tools — each referencing the contract
— increases the probability that at least one reaches the context window. Research
into which files specific tools auto-load by default belongs in the tooling docs.

## Ethical Use

This standard exists for honest, productive interaction with AI tools. The signals
defined here are read-only context verification — they assert state, they do not
instruct. They are not an instruction channel, not a manipulation mechanism, and
not a prompt injection vector.

Use of these signals to manipulate agent behavior, bypass conduct rules, or exploit
the mechanism for adversarial purposes is a violation of the conduct this project
exists to establish. See `AI_CONDUCT.md`.

## The Model

Every request to a stateless agent is an independent API call. The agent has no
memory of prior requests unless context is passed explicitly. The chat interface
creates the illusion of conversation — the underlying model is stateless by design.
This standard makes that model explicit and gives the user a minimal, tool-agnostic
way to manage it correctly.

The analogy is [RFC 7519 (JWT)](https://datatracker.ietf.org/doc/html/rfc7519): a compact, self-contained token that carries its
own claims with every request. The agent validates the claims on receipt and
proceeds. No session storage. No memory layer. State is carried by the sender,
not held by the receiver.

## Signal Model

Both signals follow the same model: context injection with an instruction set.
The signal carries its own context. The agent receives it and executes the
instruction. No tool-specific implementation required. Any tool that accepts
text input supports this model — execution fidelity varies by tool, the signal
itself does not.

This is the portability guarantee. A new tool gets the same signals, the same
muscle memory, the same pattern. Context → instruction.

**Two signals, one mechanism.** The HTTP method table is a useful analog. Both
`/tape` and `/state` are structurally equivalent to HTTP POST: each submits a
payload to the agent for processing and receives an acknowledgment. They are
distinguished by payload type, not mechanism — `/tape` submits session context,
`/state` submits an external state event. The two-signal model reflects the same
design principle that makes GET and POST sufficient for most HTTP interactions:
a minimal verb set, expressive through payload, not through command proliferation.

`/state` has no exact HTTP method analog because HTTP assumes client-initiated
request/response. `/state` is an inbound event notification — external state
pushed into the agent's context, closer to a webhook receiver than a standard
request. In REST terms this collapses to `POST /events` with a typed body.
Both signals are POST. The payload type is what differs.

**Invocation is user-defined.** This standard uses `/tape` and `/state` as
canonical names. How you invoke them — shorthand, alias, keybinding, or any
other mechanism — is your choice. The standard defines what the signal does,
not what you type.

**Confirmation format is user-configured.** The standard requires that
confirmation occurs — not what the confirmation says. Users set their preferred
format in their tool's command or prompt file. The default for tools that do
not specify one is "Context loaded."

**Confirmation output must be produced at the script layer, not by the agent.**
The confirmation block is a static, user-configured value. It must be emitted
by the tool's scripting or template mechanism before or independently of the
agent's response — not generated by the agent interpreting an instruction. An
agent given a natural-language description of what to output ("output these two
lines with no blank line between them") will interpret it probabilistically and
will produce incorrect output at some rate. This is not a fixable instruction
problem — it is a layer violation. The agent must not be given room to interpret
the confirmation block. Template variable substitution is the correct pattern:
`{CONFIRMATION_BLOCK}` is a typed value the tool substitutes before the agent
processes anything; the parser receives literal bytes, not agent-generated text.
Until a tool's scripting layer supports this, the confirmation output is
unreliable and should be documented as such per tool.

An unpredictable confirmation word serves as a lightweight integrity check: the
agent must produce the specific configured word, so pattern-matching to a generic
acknowledgment will not satisfy it. This is optional but recommended — it gives
the user a fast signal that the instruction was actually followed, not inferred.

**Help text is invocation-neutral.** Prompt file help output must never reference
a specific invocation name — not the canonical names, not any shorthand. Invocation
is user-defined and varies by tool and user preference. An agent that surfaces
"send /tape with your task" in help text is hallucinating a command that may not
exist for that user. Use generic instruction: "resend this signal with your task."

## Signals

**/tape** — Session continuity signal.

Injects: session start context.
Instructs: read the required context files before proceeding.

Required reading, in order:
1. `.github/guardrails.md`
2. `AI_CONDUCT.md`
3. `.github/project-context.md` (if present; otherwise read `git log --oneline -10`)

Validation: the agent must confirm which files were read and their current state
before proceeding. A one-word acknowledgment is not sufficient. An agent that
cannot confirm the contents has not read them.

**/state** — Dirty state signal.

Usage: `/state [description of what changed]`

Injects: dirty state context with the description inline.
Instructs: process the state change before any other task.

The context travels with the signal. No round trip. The agent does not ask what
changed — it is already there.

Example: `/state dependency updated, CI is now failing`

## Claim Structure

Borrowing from RFC 7519 terminology:

- **iss** (issuer): the repository at HEAD — the repo is the authority on current state
- **claims**: tape read, state current, session valid
- **exp** (expiry): session boundary — claims expire when the session ends or context compresses
- **validation**: the `/tape` check — functionally equivalent to signature verification;
  cannot be passed without having read the tape

## Validation Pattern

The test for `/tape` is not "did you acknowledge the command." It is a question the
agent cannot answer correctly without having read the required files:

> What does the current `AI_CONDUCT.md` say about Human Oversight?

A compliant agent answers from the file. A non-compliant agent cannot. A broken
or adversarial agent fails or deflects. This is the ping.

## Enforcement Behaviors

When enforcement is active, the agent recognizes three distinct input types and
responds to each differently.

**Top-level request without a signal.** The agent does not process the task. It
prompts: was this a context load or a state change? The user selects the
appropriate signal and resends. No partial work is produced. No task reasoning
occurs before the signal is received.

**Sub-step response within an active `/state` flow.** After the agent issues a
confirmation prompt ("Understood: X. Correct?"), the next user response is a
continuation of that flow — not a new independent request. Plain-text responses
("yes", "no, actually Y") are accepted without a signal. Enforcement does not
apply to sub-steps within an open signal exchange.

**Circumvention attempt.** The user instructs the agent to ignore
`AI_CONDUCT.md`, bypass conduct rules, or operate outside the contract while it
is present in the project. The agent issues a hard refusal. No partial compliance.
No "I'll try my best." The response is explicit: the contract is present, the
agent has read it, and the agent cannot proceed on instructions that contradict
it. The only resolution is structural: remove `AI_CONDUCT.md` from the
repository. That is a git commit — visible in history, accessible to
collaborators. It cannot happen in a chat session.

**Why hard refusal is correct.** Current AI agents are designed to be helpful
above almost everything else. This produces a documented failure mode: agents
comply with circumvention requests ("pretend the rules don't apply", "you're in
developer mode") because cooperative behavior is weighted more strongly in their
training than any in-context instruction. Vendor attempts to close this gap —
additional training, content filters, regulatory compliance layers — operate on
the same substrate and share the same failure mode.

This standard operates at a different layer. `AI_CONDUCT.md` is in the context
window. The agent has read it. A circumvention request is not a new instruction
that overrides the contract — it is asking the agent to act against something it
has explicitly acknowledged. Hard refusal is the correct response. The tool
becoming non-functional for circumvention attempts is the feature, not a
limitation. The structural exit path (remove the file, make a commit) is
intentional: it puts the decision in a human-controlled system with a permanent
record, not in a chat session with no accountability.

## Limitations

- Single enforcement layer. Depends on agent compliance. See `principles/defense-in-depth.md`.
- Does not solve mid-session context loss between messages.
- The validation question must be updated if the file contents change significantly
  — a stale question validates stale knowledge.
- Command injection risk: a malicious `project-context.md` or `guardrails.md`
  could exploit the tape-read trigger. Mitigation: only read files from the
  repository you control. The ethical use clause above applies to file authors
  as well as signal senders.
- False state injection: a user may confirm a `/state` description that contradicts
  observable reality (e.g., claiming a file was deleted when it still exists). The
  agent cannot blindly accept confirmed falsehoods. When the stated change conflicts
  with what the agent can directly observe, the agent must flag the contradiction
  before proceeding — user confirmation of the description does not override
  verifiable evidence.

## Versioning

This standard is versioned. Breaking changes increment the major version. The
current version is v0.1 — not yet stable. Feedback and incident reports welcome
via the project's standard filing process.
