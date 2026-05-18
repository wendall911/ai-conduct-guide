# 2026-05-18 — Instruction Mechanism Pattern: Broken or Absent Across Microsoft-Hosted Tools and Cursor

**Tools:** GitHub Copilot (VS Code and IntelliJ), Cursor

**What was investigated:** Whether instruction files (`.github/copilot-instructions.md`,
`.cursorrules`, `.cursor/rules/`) reliably inject behavioral context at session
start across major AI coding tools — the documented adoption path for this contract.

**What was found:**

*GitHub Copilot / VS Code:* `copilot-instructions.md` instruction-following is
documented as automatic but was reported broken. The issue was confirmed and
closed as "not planned" by Microsoft (microsoft/vscode#292906). The documented
mechanism for establishing a behavioral contract at session start does not
function as documented and will not be fixed.

*GitHub Copilot / IntelliJ:* Instruction file support does not exist. Support
for `.github/copilot-instructions.md` is a planned but unimplemented feature
(microsoft/copilot-intellij-feedback#413). Even manual injection of instructions
is inconsistently followed (microsoft/copilot-intellij-feedback#619). Agent mode
specifically ignores custom instructions (microsoft/copilot-intellij-feedback#258).
No hooks system is available for the Copilot plugin in IntelliJ.

*Cursor:* `.cursorrules` is intentionally ignored in agent mode — the default
mode — with no warning to the user. This is a documented architectural decision.
The replacement format (`.cursor/rules/*.mdc` with `alwaysApply: true`) requires
a session reload to apply. The SessionStart hook exists but `additionalContext`
injection is broken — context is accepted but never injected into the agent's
system context (multiple confirmed reports).

**Root cause:** The instruction file pattern — add a file to the project and the
tool reads it — is a documentation commitment without corresponding enforcement
in the tool implementation. In each case, the mechanism was either never
implemented (IntelliJ Copilot), deprecated by design without warning (Cursor
`.cursorrules` in agent mode), or broken and left broken (VS Code Copilot). The
pattern is consistent across tools and consistent with prioritizing token
throughput over correct tooling configuration behavior.

**Why this finding is notable:** Projects that follow the documented adoption
path — add `AI_CONDUCT.md` to the instruction file — believe they have
established the behavioral contract. They have not. The contract is present in
the repository but not in the agent's context. This is a false adoption that
provides no protection while creating the appearance that protection exists.

**Contract clause:** Defense in Depth — "A single-layer rule is fragile. Do not
treat this document as sufficient enforcement on its own." This finding is direct
evidence for that clause. The instruction file is the documented single layer.
It does not work. Enforcement requires additional layers.

**Amendment:** Tooling documentation updated to remove instruction files as
reliable session-start mechanisms. VS Code Copilot: SessionStart hook documented
as replacement. Cursor: `.cursor/rules/*.mdc` with `alwaysApply: true` documented
as working format; SessionStart hook noted as broken for context injection.
IntelliJ Copilot: instruction file guidance removed; Claude Code CLI documented
as the viable path.

**Citations:**
- microsoft/vscode#292906 — `copilot-instructions.md` closed as "not planned":
  https://github.com/microsoft/vscode/issues/292906
- microsoft/copilot-intellij-feedback#413 — instruction file support not
  implemented: https://github.com/microsoft/copilot-intellij-feedback/issues/413
- microsoft/copilot-intellij-feedback#258 — agent mode ignores custom
  instructions: https://github.com/microsoft/copilot-intellij-feedback/issues/258
- microsoft/copilot-intellij-feedback#619 — instructions ignored even when
  manually added: https://github.com/microsoft/copilot-intellij-feedback/issues/619
- Cursor forum: `.cursorrules` silently ignored in agent mode:
  https://forum.cursor.com/t/cursorrules-file-silently-ignored-in-agent-mode-with-no-warning/152046
- Cursor forum: sessionStart hook `additionalContext` not injected:
  https://forum.cursor.com/t/sessionstart-hook-additional-context-is-never-injected-into-agents-initial-system-context/158452
- VS Code Agent Hooks documentation:
  https://code.visualstudio.com/docs/copilot/customization/hooks

**Classification:** Research-based finding. Primary sources are official GitHub
issue trackers and Cursor's official community forums.
