# GitHub Copilot

## Tool Overview

GitHub Copilot is Microsoft's AI coding assistant, available as a VS Code
extension and in other editors. Instructions are read from
`.github/copilot-instructions.md`.

## Known Failures

- Instruction compliance is non-deterministic by design. GitHub's own
  documentation states: "Due to the non-deterministic nature of AI, Copilot
  may not always follow your custom instructions in exactly the same way
  every time they are used."
  Source: https://docs.github.com/en/copilot/concepts/prompting/response-customization

- Instructions are injected as context, not enforced as constraints. There
  is no blocking mechanism equivalent to Claude Code's `PreToolUse` hooks.

- Injects `Co-Authored-By: GitHub Copilot <noreply@github.com>` in some
  versions. Default has been changed to off following user complaints but
  behavior varies by version.

## Configuration

Disable co-author injection (VS Code):

```json
// settings.json
{
    "git.addAICoAuthor": "off"
}
```

Add system-level hook as backstop regardless of setting (see claude-code.md
for hook setup — the same hook covers all tools).

## Evaluation

Non-deterministic compliance is not a bug — it is a documented product
decision. The capability for hard enforcement exists in agent tooling
(Claude Code's `PreToolUse` hooks are proof). Its absence in Copilot
is a design choice, not a technical limitation. Projects that require
reliable instruction compliance should account for this.

Full incident documentation: see INCIDENTS.md.
