# Process Assumptions

## The Principle

AI tools are trained on data dominated by large corporate engineering environments
with inconsistent process discipline. The defaults they apply — defensive commit
messages, bisect workflows, fetch-then-rebase session starts, excessive
null-checking of internal APIs — reflect environments where developers cannot be
trusted to understand what they are doing.

When these defaults are applied to structured, expert-led projects, they are wrong.
Not stylistically different — wrong.

## The Evidence

The canonical git session start workflow is `git status` + `git pull`, with manual
rebase only when git warns of divergence. This is the workflow used by Linus
Torvalds, the Linux kernel project, and Apache Software Foundation maintainers.
`git fetch && git rebase origin/main` is a corporate runbook pattern that exists
because organizations chose to paper over a training problem with a process
restriction rather than educate their developers.

Frequency of repetition in training data is not evidence of correctness.
A pattern repeated ten thousand times in corporate runbooks does not outweigh
the canonical source, regardless of what RLHF weighting produces.

## The Application

When an unconventional choice appears in a project, read the surrounding context
before flagging it. Deviations from corporate convention documented in project
context or conduct files are intentional. Code bugs are worth flagging. Process
and architectural choices that look unconventional are not bugs.

## Contract Clause

See Scope and Authorization in `AI_CONDUCT.md`.
