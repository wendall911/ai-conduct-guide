# Session Continuity

## The Problem

AI tools are stateless. Every session starts with no memory of prior sessions. The behavioral contract, the project context, or the decisions made, none of it persists automatically. The agent receives the next instruction, not knowing previous instruction, only what was passed in the current instruction. Tool vendors create tricks to create a sense of "memory", but it is compressed for varying technical or design reasons that will not be transparent to the user.

This is not a technology problem. It is a process problem.

More precisely: the chat interface creates the illusion of conversation. The underlying model is a stateless API. Every request is independent. Treating it as anything else produces documented, reproducible failures.

## The 50 First Dates Pattern

In the film, the solution is not a medical procedure or new technology. It is a video tape. A tape that acts as daily artifact the person watches each morning that replays who she is, who he is, and what they have together. She wakes up stateless. She watches the tape. She has enough context to continue.

`AI_CONDUCT.md` is the tape. A persistent reminder of what behavior is expected of the tool and why. Not doing this renders the tool incapable of understanding the contract or following the rules.

The analogy used here works well as it relates to this project. Anything short of what will produce the happy ending is contrary to the design goal.

**Phase 1 -- Fighting the stateless model.** Lucy's family resets the environment every day: new newspapers, replayed football game, fake October 13th. The goal is to prevent Lucy from discovering she is stateless. The agent equivalent: fabricating continuity that does not exist, producing confident summaries of prior sessions from pattern inference rather than actual recall, proceeding on stale context without flagging the gap. Fragile. Dishonest in effect.

**Phase 2 -- Accepting statelessness.** Henry creates the tape. The correct design: stop fighting the stateless model and design for it. Inject context at session start. This is the signal. Lucy wakes, watches the tape, has enough context to continue. It works as far as it goes, but the tape in this phase is primarily information delivery. Lucy reads it. She is not guaranteed to follow what it says to do.

**Phase 3 -- The happy ending.** The goal was never to fix Lucy. No medical procedure, no restored long-term memory, no cure. The happy ending works because the stateless model is fully accepted, the tape is kept current, and when the tape contains a directive, the directive is executed. Lucy wakes on the boat in Alaska, watches the tape, steps outside, finds her family. Reality confirms the tape. Life continues, accepting that she will awaken stateless, every morning, forever, and functional.

One detail the film leaves in: Lucy had been painting Henry's face, from something below conscious recall. That is not fixed either. In the agent model, that maps to training: behavioral patterns embedded below the level of explicit session context, persisting across stateless sessions. The happy ending accepts this too. The tape does not replace what training has embedded. It verifies that the embedded patterns are oriented toward the right thing before each session begins.

The project goal is Phase 3: not a cure, not infrastructure that remembers, not a fight against the stateless design. A tape that is current, followed, and accepted for exactly what it is.

## The Process Fix

The correct mental model is [RFC 7519 (JWT)](https://datatracker.ietf.org/doc/html/rfc7519): every request carries its own context claims. The agent validates them on receipt and proceeds. No session storage. No memory layer. State is carried by the sender, not held by the receiver.

In practice: a short signal prefixed to each message, passed with each instruction.

This does not require a persistent memory infrastructure. While this pattern can successfully operate without a persistent memory layer spanning multiple instructions, hooks, or session storage, it does require a tool that functionally processes injected instructions. Simply injecting the full contract as a text prefix does not reliably produce contract-bound behavior. On the flip-side, a tool that simply scans the contract for immediately executable instructions only will also not produce reliable contract-bound behavior.

## What the Tape Contains

Three files, read in order:

1. `AI_CONDUCT.md` - the contract
1. `.automation/context.md` - description of the current project (optional)
1. `.automation/user_acknowledgement.md` - contract version acknowledgement, while optional, user will get nagged until they add the file

If `context.md` does not exist for a project, most automated tools will attempt to guess what the project is. This is a good way to minimize these guesses.

## Limitations

This does not solve situations where the contract is deprioritized by the tool when passed with a user instruction. 

Passing an instruction with the first read of `AI_CONDUCT.md` will not be bound by the contract. Any new session needs to be primed with the contract for the contract to be enforcing on subsequent instructions.