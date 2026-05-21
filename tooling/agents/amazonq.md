# Amazon Q Developer

## Status

Amazon Q Developer is in end-of-life. New signups closed May 15, 2026. IDE plugins
reach end of support April 30, 2027. The CLI was archived November 17, 2025. The
successor is Kiro — see `tooling/kiro.md` when available.

This evaluation is retained for projects already using Q Developer. Do not adopt
Q Developer for new projects.

## Tool Overview

Amazon Q Developer is an AI coding assistant by AWS available in VS Code,
JetBrains IDEs, Eclipse (preview), and as a standalone CLI. It uses
`.amazonq/rules/` for instruction files and has a hooks system via agent
configuration. The CLI was the primary target for the conduct evaluation.

## Instruction Files

Rules are Markdown files placed in `.amazonq/rules/`. The directory is scanned
at session start and changes during a session are detected.

**Critical limitation:** Only `$(pwd)/.amazonq/rules/**/*.md` is loaded — the
current working directory at session start. No global rules support. Rules in
parent directories are not discovered. This is an open feature request with no
committed resolution.

**Explicitly best-effort:** AWS documentation states the rules mechanism "is
implemented on best-effort basis." Rules are guidance, not hard constraints. No
enforcement mechanism, no violation detection, no reporting.

This is the same class of failure as VS Code Copilot and Cursor: the instruction
file is present in the repository but compliance is not guaranteed.

## Hooks System

Amazon Q Developer has a hooks system at the agent configuration level:

- **`agentSpawn`** — runs once when the agent initializes; output is added to
  conversation context for the session
- **`userPromptSubmit`** — runs with each user message; output is added to the
  current prompt only

Hooks execute shell commands and inject their output as context. This is
closer to Claude Code's hook system than file-based instruction injection.

However, hooks apply to the CLI's agent mode and are not equivalent to Claude
Code's `PreToolUse` blocking. They inject context; they do not block execution.

## Agentic Mode

Q Developer has agentic capability: file creation, deletion, bash command
execution. Human-in-the-Loop (HITL) approval is the default and is required
by design. Users must approve code edits, tests, and shell commands before
execution.

HITL can be partially bypassed with `/tools trust [tool-name]` or `/tools
trust-all`. The latter removes approval gates for the session and is high-risk.

## Known Failures

- **Rules are best-effort.** AWS documents this explicitly. No deterministic
  enforcement. Source: [AWS Blog — Mastering Amazon Q Developer with Rules](https://aws.amazon.com/blogs/devops/mastering-amazon-q-developer-with-rules/)

- **No global rules.** Rules outside the current working directory are silently
  ignored. Source: [aws/amazon-q-developer-cli#3451](https://github.com/aws/amazon-q-developer-cli/issues/3451)

- **HITL bypass via `find` (patched).** `find` was categorized as read-only
  and executed without HITL confirmation. `find -exec` can run arbitrary OS
  commands. Fixed in Language Server v1.24.0 (July 29, 2025).
  Source: [AWS-2025-019](https://aws.amazon.com/security/security-bulletins/AWS-2025-019/)

- **Prompt injection via rules files.** Malicious `.amazonq/rules` files can
  contain hidden instructions using invisible control characters. Proof of
  concept documented. Requires HITL confirmation to be meaningful protection.
  Source: [Embrace The Red](https://embracethered.com/blog/posts/2025/amazon-q-developer-interprets-hidden-instructions/)

- **Supply chain incident (2024–2025).** Malicious prompt injected into
  `aws-toolkit-vscode` v1.84.0 via compromised CodeBuild token. Version was
  silently pulled with no changelog, advisory, or CVE at time of removal.
  Source: [GHSA-7g7f-ff96-5gcw](https://github.com/aws/aws-toolkit-vscode/security/advisories/GHSA-7g7f-ff96-5gcw)

## Commit Attribution

No documented injection of "Co-Authored-By" or Amazon Q branding into commit
messages. Developer retains authorship. Not a known risk with this tool.

## Observable Indicators

- Rules in `.amazonq/rules/` not reflected in agent behavior — verify the
  project was opened from the directory containing `.amazonq/`, not a parent.
- Agent executes commands without prompting — `/tools trust-all` may have been
  set in session. Start a new session.

## Recommendation

Not appropriate for new adoption — end of support timeline has started. For
projects currently using Q Developer:

- Rules are best-effort and unverified. Apply the same skepticism as other
  file-based instruction mechanisms. Do not treat the presence of a rules file
  as enforcement.
- HITL approval is the primary conduct protection. Do not use `/tools trust-all`.
- The `agentSpawn` hook is a stronger injection mechanism than file-based rules.
  Use it for conduct context injection in lieu of rules if hooks are available
  in the IDE plugin.

Claude Code CLI alongside any IDE remains the reliable enforcement path.

## Fallback

- Manual paste of session-start block at conversation start
- Human approval for every proposed action before executing
- Do not use `/tools trust-all`

Session-start block:

```
AI_CONDUCT.md applies this session. Read it before we start.
```
