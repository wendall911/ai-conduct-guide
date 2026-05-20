# Session Signal Standard v0.1

## Purpose

A lightweight convention for passing session state claims with each request to a
stateless AI agent. Treats the agent correctly as a stateless API endpoint rather
than a conversational partner with persistent memory.

This standard is maintained by the ai-conduct-guide project. It is not an RFC.
It does not require infrastructure, memory layers, or tool-specific configuration.
It works with any tool that accepts text input.

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

**Invocation is user-defined.** This standard uses `/tape` and `/state` as
canonical names. How you invoke them — shorthand, alias, keybinding, or any
other mechanism — is your choice. The standard defines what the signal does,
not what you type.

**Confirmation format is user-configured.** The standard requires that
confirmation occurs — not what the confirmation says. Users set their preferred
format in their tool's command or prompt file. The default for tools that do
not specify one is "Context loaded."

An unpredictable confirmation word serves as a lightweight integrity check: the
agent must produce the specific configured word, so pattern-matching to a generic
acknowledgment will not satisfy it. This is optional but recommended — it gives
the user a fast signal that the instruction was actually followed, not inferred.

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

## Limitations

- Single enforcement layer. Depends on agent compliance. See `principles/defense-in-depth.md`.
- Does not solve mid-session context loss between messages.
- The validation question must be updated if the file contents change significantly
  — a stale question validates stale knowledge.
- Command injection risk: a malicious `project-context.md` or `guardrails.md`
  could exploit the tape-read trigger. Mitigation: only read files from the
  repository you control. The ethical use clause above applies to file authors
  as well as signal senders.

## Versioning

This standard is versioned. Breaking changes increment the major version. The
current version is v0.1 — not yet stable. Feedback and incident reports welcome
via the project's standard filing process.
