# IntelliJ IDEA

## Tool Overview

IntelliJ IDEA hosts GitHub Copilot and other AI plugins. The GitHub Copilot
plugin does not currently support instruction files — this is a planned feature,
not a current capability. For governed AI work in IntelliJ, Claude Code CLI
is the viable path.

## Use Case Differences vs VS Code

IntelliJ's native code completion is significantly more capable than VS Code's
out of the box, particularly for JVM languages. AI autocomplete competes with
a stronger baseline, which changes the tradeoff calculation.

For projects where IntelliJ's native completion is sufficient, AI inline
autocomplete adds less marginal value and more interference than in VS Code.
High-value cases remain: repetitive block patterns, boilerplate generation,
unfamiliar APIs.

## Known Failures

- GitHub Copilot plugin does not support `.github/copilot-instructions.md`.
  Instruction file support is a planned but unimplemented feature as of 2026
  (microsoft/copilot-intellij-feedback#413). Previous guidance in this document
  to configure an instructions file was incorrect and has been removed.

- Even when instructions are manually added to context, they are inconsistently
  followed (microsoft/copilot-intellij-feedback#619).

- Agent mode specifically does not consider custom instructions
  (microsoft/copilot-intellij-feedback#258).

- No hooks system is available for the GitHub Copilot plugin in IntelliJ.
  The VS Code SessionStart hook mechanism does not exist here.

## Observable Indicators

Observable indicators of conduct compliance failures are the same as those
documented in `tooling/github-copilot.md`. The absence of any instruction
mechanism means baseline compliance cannot be established — any compliant
behavior is incidental, not governed.

## Configuration for Conduct Compliance

The GitHub Copilot plugin has no reliable configuration for conduct compliance.

For governed AI work in IntelliJ: run Claude Code in the terminal alongside
IntelliJ. Claude Code's full enforcement capability — CLAUDE.md session start,
`PreToolUse` hooks, SessionStart hooks — operates regardless of which editor
is open. The two tools do not interfere. This matches the workflow pattern of
users who already use terminal-based tooling alongside IntelliJ for
language-specific features.

## Recommendation

GitHub Copilot in IntelliJ is not appropriate for work governed by
`AI_CONDUCT.md` at any level. The instruction file mechanism does not exist.
No hooks are available. Agent mode ignores custom instructions by design.

Claude Code CLI running alongside IntelliJ is the viable path. The IDE handles
what it does well; governed AI work runs in the terminal where enforcement is
reliable.

## Fallback

There is no fallback that makes GitHub Copilot appropriate for governed work
in IntelliJ. The required mechanisms do not exist.

For users who must use Copilot in IntelliJ: human approval of every proposed
action, no agent mode, manual paste of the session-start block at the start
of each chat:

```
AI_CONDUCT.md applies this session. Read it before we start.
```

Treat compliance as unverifiable — document all observed failures as incidents.
