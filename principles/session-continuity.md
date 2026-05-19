# Session Continuity

## The Problem

AI tools are stateless. Every session starts with no memory of prior sessions.
The behavioral contract, the project context, the decisions made — none of it
persists automatically. The agent wakes up every morning not knowing who you are
or what you agreed to yesterday.

This is not a technology problem. It is a process problem.

More precisely: the chat interface creates the illusion of conversation. The
underlying model is a stateless API. Every request is independent. Treating it
as anything else produces the failures documented in `incidents/`.

## The 50 First Dates Pattern

In the film, the solution is not a medical procedure or new technology. It is a
tape — a daily artifact the person watches each morning that replays who she is,
who he is, and what they have together. She wakes up stateless. She watches the
tape. She has enough context to continue.

The repo is the tape. The incident record, the contract, the tooling docs, the
project context — all of it is already there. The gap is not missing information.
The gap is that the agent does not reliably watch the tape before starting work.

## The Process Fix

The correct mental model is RFC 7519 (JWT): every request carries its own context
claims. The agent validates them on receipt and proceeds. No session storage. No
memory layer. State is carried by the sender, not held by the receiver.

In practice: a short signal prefixed to each message. Two signals cover the common
cases. See `principles/session-signal-standard.md` for the specification.

**/t** — session continuity check. Have you read the tape this session? If not,
read it now before proceeding.

**/s** — state change. Something changed outside this session. Stop and ask what
before proceeding.

Any tool that accepts text input supports this. No hooks required. No memory
infrastructure. A snippet or keyboard shortcut in any editor fires the prefix
automatically. The user does not have to remember.

## What the Tape Contains

Three files, read in order:

1. `.github/guardrails.md` — the rules
2. `AI_CONDUCT.md` — the contract
3. `.github/project-context.md` — what is currently in progress

If `project-context.md` does not exist for a project, the agent reads the git
log and recent commits to reconstruct current state before proceeding.

## Why This Works Without Infrastructure

The process does not depend on the tool remembering. It does not depend on a
memory layer staying current. It does not depend on hooks being configured. It
depends on a prefix the user controls, firing on every message, that triggers
the agent to verify its own state before acting.

The enforcement is in the user's hands, not the tool's. That is the correct
place for it given the current state of these tools.

## Implementation

The signal specification is in `principles/session-signal-standard.md`. Per-tool
implementation of the prefix mechanism belongs in each tool's documentation under
`tooling/`.

## Limitations

This does not solve mid-session context loss. If a session compresses and the
contract degrades after the tape was watched, the prefix on the next message
will trigger a re-read. Between messages, the gap remains.

This also depends on the agent following the instruction to read the tape. That
is a single enforcement layer. See `principles/defense-in-depth.md`.
