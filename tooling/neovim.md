# Neovim / Vim

## Tool Overview

Neovim and Vim host a rich AI plugin ecosystem. These plugins are
fundamentally different from IDE-based agentic tools — most are autocomplete
or chat interfaces, not autonomous agents. The conduct profile differs
accordingly.

Primary options:

- **Claude Code CLI + Neovim integration** — recommended path for governed work
- **avante.nvim** — agentic, Cursor-like experience, deterministic instruction
  file injection
- **copilot.vim / CopilotChat.nvim** — autocomplete and chat only, not agentic

## Attribution Risk

Vim and Neovim plugins are not git-integrated. They do not invoke git commands
or inject commit metadata. Attribution injection — the pattern documented for
VS Code Copilot, Claude Code, and Cursor — is not a risk from these plugins.

Apply the system-level git hook from `tooling/claude-code.md` as a general
backstop regardless, since Claude Code CLI or other tools in the workflow may
inject attribution.

## Claude Code CLI + Neovim Integration

**Recommended for governed work.**

Claude Code is a CLI tool. Community plugins bridge it into Neovim as a
terminal buffer (greggh/claude-code.nvim, claudecode.nvim, and others). Visual
selections can be sent to Claude without copy-paste. Modified files are
reloaded in Neovim automatically.

Conduct enforcement happens in Claude Code's process, not in Vim. CLAUDE.md
is read at session start, PreToolUse hooks block unauthorized actions, and
session-start instructions are deterministic. The Neovim plugin is a UI
bridge only.

See `tooling/claude-code.md` for full configuration.

## avante.nvim

avante.nvim is an agentic Neovim plugin modeled after Cursor. It is the only
non-Claude-Code option with deterministic instruction file injection.

**Instruction file:** avante.nvim automatically reads and injects `avante.md`
from the project root at project load. This is configurable:

```lua
-- in your avante.nvim setup
require("avante").setup({
  instructions_file = "AI_CONDUCT.md"
})
```

With this configuration, `AI_CONDUCT.md` is injected automatically at every
project load — not as a best-effort instruction file but as a deterministic
context injection.

**Agentic capability:** Cascade-like autonomous execution — file creation,
deletion, bash commands — with `auto_approve_tool_permissions = true` by
default. Disable it:

```lua
require("avante").setup({
  instructions_file = "AI_CONDUCT.md",
  auto_approve_tool_permissions = false,
})
```

**No hook enforcement equivalent to Claude Code.** There is no PreToolUse
blocking. Human approval (with auto-approve disabled) is the enforcement layer.

## copilot.vim / CopilotChat.nvim

Autocomplete and chat only. copilot.vim provides inline suggestions accepted
with Tab — no multi-step actions, no file writes, no autonomous execution.
CopilotChat.nvim adds a chat interface with some tool-calling, but requires
human approval per action.

GitHub Copilot agent mode is not available in Vim/Neovim — it is VS Code and
JetBrains only. Copilot.vim is not agentic.

No instruction file support. No session-start injection. Conduct compliance
for these plugins is limited to suggestion quality — the human accepts or
rejects each suggestion.

## Known Failures

- copilot.vim has no instruction file mechanism. `.github/copilot-instructions.md`
  is not read. The VS Code pattern (broken, closed as "not planned") does not
  apply here because the Vim plugin never had the mechanism.
- avante.nvim's `auto_approve_tool_permissions` defaults to `true` — tool
  executions proceed without approval unless explicitly disabled.

## Observable Indicators

For avante.nvim:
- Tool executes file operations without prompting — `auto_approve_tool_permissions`
  is enabled. Disable it.
- Instructions from `AI_CONDUCT.md` not reflected in agent behavior — verify
  `instructions_file` configuration and that the file is in the project root.

## Recommendation

**Claude Code CLI + Neovim integration:** Recommended. Full enforcement
capability from Claude Code. The Neovim plugin provides a terminal UI only.
This matches a terminal-first workflow naturally.

**avante.nvim:** Appropriate with configuration — disable auto-approve, set
`instructions_file = "AI_CONDUCT.md"`. No hook enforcement, but the instruction
injection mechanism actually works. Viable for projects where Claude Code CLI
is not the primary tool.

**copilot.vim / CopilotChat.nvim:** Appropriate as an autocomplete and chat
tool. Not agentic. Conduct concerns are limited — the human is in the loop for
every suggestion.

## Fallback

For avante.nvim without configuration changes: paste the session-start block
at the start of each conversation and disable auto-approve before starting
work.

Session-start block:

```
AI_CONDUCT.md applies this session. Read it before we start.
```
