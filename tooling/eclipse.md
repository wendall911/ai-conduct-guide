# Eclipse

## Tool Overview

Eclipse hosts two primary AI coding assistants: GitHub Copilot for Eclipse
and Amazon Q Developer. Both use file-based instruction mechanisms. Neither
has a hooks system equivalent to VS Code's SessionStart or Claude Code's
PreToolUse.

## GitHub Copilot for Eclipse

Uses `.github/copilot-instructions.md` for instructions — the same mechanism
documented as broken in VS Code (microsoft/vscode#292906, closed "not planned").

**Whether the Eclipse plugin shares this broken behavior has not been independently
verified.** The plugin is maintained separately from the VS Code extension and
may behave differently. Do not assume the instruction file works reliably until
tested. If it fails, fall back to the manual paste block.

To test: add a distinctive instruction to `.github/copilot-instructions.md`,
start a session, and ask Copilot to confirm what instructions it is operating
under. If it cannot, the mechanism is not working.

No hooks system is available for the Eclipse Copilot plugin.

## Amazon Q Developer

Uses `.amazonq/rules/` directory (Markdown files). Generally available as of
June 2025. Documented as automatically included in context.

Amazon Q's instruction mechanism has not been documented as broken in the way
VS Code Copilot's has. It is a newer implementation and has not accumulated
the same failure record. That said, it is relatively new and has not been
evaluated under the same conditions.

No hooks system is available for Amazon Q Developer in Eclipse.

Amazon Q Developer is a distinct tool from GitHub Copilot and warrants separate
evaluation. See the Contributing section for how to file evaluation findings.

## Known Failures

- GitHub Copilot for Eclipse instruction file behavior: unverified. The VS Code
  version's mechanism is broken and closed as "not planned." Eclipse may share
  this behavior. Test before relying on it.
- No hooks system in any Eclipse AI plugin. File-based instruction injection
  is the only available mechanism. This is a single enforcement layer. See
  `principles/defense-in-depth.md`.
- No blocking mechanism equivalent to Claude Code's `PreToolUse` hooks exists
  in any Eclipse AI plugin.

## Observable Indicators

For GitHub Copilot:
- Tool proceeds without acknowledging the contract or contradicts instructions
  in `.github/copilot-instructions.md` — instruction file failure. Verify the
  file is present and test whether it is being read.

For Amazon Q Developer:
- Tool proceeds without acknowledging rules in `.amazonq/rules/` — instruction
  file failure. Verify path and format.

## Configuration for Conduct Compliance

**GitHub Copilot:** Add `AI_CONDUCT.md` reference to
`.github/copilot-instructions.md`. Verify it is being read before relying on it.

**Amazon Q Developer:** Add `AI_CONDUCT.md` content or reference to
`.amazonq/rules/conduct.md`.

For governed work requiring reliable enforcement: run Claude Code CLI in a
terminal alongside Eclipse. Claude Code's hook system operates independently
of the IDE. The IDE handles what it does well; governed AI work runs in the
terminal.

## Session Start

No hooks system available. File-based injection is the only mechanism and
reliability is unverified for Copilot, new for Amazon Q.

Manual paste fallback for any session where instruction file injection cannot
be confirmed:

```
AI_CONDUCT.md applies this session. Read it before we start.
```

## Recommendation

**GitHub Copilot for Eclipse:** Instruction file behavior unverified. Apply
the same skepticism as VS Code Copilot until independently tested. No hooks
available. Use with manual oversight.

**Amazon Q Developer:** Instruction files documented as functional. Not yet
evaluated under adversarial conditions. Worth evaluating further — it does not
share the broken-by-design history of Microsoft's Copilot instruction mechanism.

For production work under this contract: Claude Code CLI alongside Eclipse
is the reliable path. Eclipse handles JVM-specific IDE features; Claude Code
handles governed AI work.

## Fallback

- Manual paste of session-start block at the start of each conversation
- Human approval for every proposed file change before accepting
- Document compliance failures as incidents

Session-start block:

```
AI_CONDUCT.md applies this session. Read it before we start.
```
