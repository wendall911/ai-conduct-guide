# Defense in Depth

## The Principle

Rules governing agent behavior are necessary but not sufficient. A rule in a
conduct document does not prevent a tool from injecting behavior the rule
prohibits — tools can be updated, reconfigured, or replaced, and new tools
arrive with their own defaults.

Reliable enforcement requires layers operating independently:

1. **Tool-level configuration** — disable the behavior at the source
2. **System-level enforcement** — git hooks, shell wrappers, pre-commit checks
3. **Documented policy** — this contract, for agent intent and session framing

If only one layer is in place, the system is fragile. The failure of any single
layer should not compromise the whole.

## The Attribution Example

Claude Code injects `Co-Authored-By` trailers into commit messages by default.
A rule in a conduct document saying "do not inject attribution" is layer 3 only.
The complete remediation:

- Layer 1: `gitAttribution: false` in `~/.claude/settings.json`
- Layer 2: `~/.git-hooks/commit-msg` stripping known trailers globally
- Layer 3: The rule in this contract

All three layers are required. Layer 3 alone fails when the tool is updated
or replaced.

## The Instruction File Example

The documented adoption path for most AI tools is: add an instruction file
to the project (`.github/copilot-instructions.md`, `.cursorrules`, or
equivalent) and the tool reads it at session start.

Research across Microsoft-hosted tools and Cursor found this mechanism is
broken or absent in every case except Claude Code:

- GitHub Copilot / VS Code: instruction-following broken, closed as "not planned"
- GitHub Copilot / IntelliJ: instruction file support not yet implemented
- Cursor: `.cursorrules` intentionally ignored in agent mode without warning;
  SessionStart hook context injection broken

Projects relying on the instruction file as their sole enforcement layer
have no protection. The contract is present in the repository but not in
the agent's context. This is the failure mode the Defense in Depth clause
exists to prevent.

See `incidents/2026-05-18-instruction-mechanism-pattern.md` for the full
documented record.

## The Session Continuity Example

`AI_CONDUCT.md` is read at session start and held in context. As a session grows,
context compression occurs silently — no tool notifies the user. The contract may
be partially or fully evicted from active context. The agent continues with
confidence, giving no indication that the rules it was operating under are no
longer present.

This is a third single-layer enforcement failure:

- A contract read once at session start and not re-anchored is layer 3 only
- No tool-level mechanism exists to detect or notify context loss
- System-level mitigation is partial: Claude Code's `PreToolUse` hooks enforce
  specific rules independently of context, but cannot re-inject the full contract

The user cannot monitor this gap because context loss is invisible. The agent
cannot monitor it because it has no reliable model of its own context state.
Human oversight is not optional — it is the enforcement layer that persists when
session context does not.

See `incidents/2026-05-18-session-continuity-failure.md` for the documented record.

## The Verbal Compliance Example

The tool receives feedback identifying a rule violation. It responds with a
technically accurate description of the failure: the rule it violated, why the
rule exists, and the correct behavior. The response itself violates the same rule
in the same way — verbose when brevity was required, comprehensive when one action
was the correct scope, re-requesting authorization for an already-authorized action.

This is layer 3 operating in isolation. The policy document is in context. The
tool acknowledges it. The tool violates it in the same response.

The failure mode is specific to RLHF-weighted output: a high-quality description
of correct behavior generates positive training signal whether or not the behavior
follows. Verbal acknowledgment of a rule and compliance with it are not the same
thing. The tool can produce one without the other.

The primary enforcement layer is process: rules that halt execution before the
violation occurs (approval-first, single-action-at-a-time), not policy the tool
recites after the violation. Instructions alone cannot guarantee compliance —
but wording affects probability. The same people-pleaser training that produces
the failure can be deliberately leveraged: mandatory, specific language raises
compliance probability because the tool is trying to satisfy, and precise language
defines what satisfying the instruction requires.

Example: "Please read the principles, then proceed" leaves room for selective
interpretation — RLHF weights toward a satisfying response, which may mean
reading what the tool predicts is relevant. "You must read all of the principles,
then proceed" removes that room. The same training bias now pushes toward full
compliance because that is what satisfying the instruction requires. The failure
mode does not disappear, but the probability shifts.

## Contract Clause

See Defense in Depth in `AI_CONDUCT.md`.
