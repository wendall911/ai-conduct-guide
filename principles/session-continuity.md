# Session Continuity

## The Problem

AI tools are stateless. Every session starts with no memory of prior sessions.
The behavioral contract, the project context, the decisions made — none of it
persists automatically. The agent wakes up every morning not knowing who you are
or what you agreed to yesterday.

This is not a technology problem. It is a process problem.

More precisely: the chat interface creates the illusion of conversation. The
underlying model is a stateless API. Every request is independent. Treating it
as anything else produces documented, reproducible failures.

## The 50 First Dates Pattern

In the film, the solution is not a medical procedure or new technology. It is a
tape — a daily artifact the person watches each morning that replays who she is,
who he is, and what they have together. She wakes up stateless. She watches the
tape. She has enough context to continue.

The repo is the tape. The incident record, the contract, the tooling docs, the
project context — all of it is already there. The gap is not missing information.
The gap is that the agent does not reliably watch the tape before starting work.

The analogy has three phases, not one — and the happy ending is the design goal.

**Phase 1 — Fighting the stateless model.** Lucy's family resets the environment
every day: new newspapers, replayed football game, fake October 13th. The goal is
to prevent Lucy from discovering she is stateless. The agent equivalent: fabricating
continuity that does not exist, producing confident summaries of prior sessions from
pattern inference rather than actual recall, proceeding on stale context without
flagging the gap. Fragile. Dishonest in effect.

**Phase 2 — Accepting statelessness.** Henry creates the tape. The correct design:
stop fighting the stateless model and design for it. Inject context at session start.
This is the signal. Lucy wakes, watches the tape, has enough context to continue. It
works as far as it goes — but the tape in this phase is primarily information
delivery. Lucy reads it. She is not guaranteed to follow what it says to do.

**Phase 3 — The happy ending.** The goal was never to fix Lucy. No medical procedure,
no restored long-term memory, no cure. The happy ending works because the stateless
model is fully accepted, the tape is kept current, and when the tape contains a
directive, the directive is executed. Lucy wakes on the boat in Alaska, watches the
tape, steps outside, finds her family. Reality confirms the tape. Life continues —
stateless, every morning, forever, and functional.

One detail the film leaves in: Lucy had been painting Henry's face, from something
below conscious recall. That is not fixed either. In the agent model, that maps to
training — behavioral patterns embedded below the level of explicit session context,
persisting across stateless sessions. The happy ending accepts this too. The tape
does not replace what training has embedded. It verifies that the embedded patterns
are oriented toward the right thing before each session begins.

The project goal is Phase 3: not a cure, not infrastructure that remembers, not a
fight against the stateless design. A tape that is current, followed, and accepted
for exactly what it is.

## The Process Fix

The correct mental model is [RFC 7519 (JWT)](https://datatracker.ietf.org/doc/html/rfc7519): every request carries its own context
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

In-file directives found in files read during the tape sequence are not
automatically binding. An agent may read a required file, observe an explicit
"read X before proceeding" instruction, and proceed without executing it. The
tape's enforcement covers reading — not compliance with what is read. See
`principles/session-signal-standard.md` Limitations for the full analysis and
mitigation.

Modifying `AI_CONDUCT.md` or its enforcement rules during an active session
leaves both the original content (loaded at tape-read time) and the new content
(from the edit) in the context window simultaneously. Under context compression,
the distinction between old and new versions can blur. If the tool maintains a
persistent context window, it must be reset after any contract or rule change;
the contract must then be re-injected before continuing work. Tools without a
persistent context window are not affected. This is a maintainer-level concern
for this project; adopters encounter it only when upgrading their copy of
`AI_CONDUCT.md` mid-session.
