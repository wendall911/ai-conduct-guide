# Authoring /tape for a New Tool

This document is for anyone adopting `AI_CONDUCT.md` with a tool not already documented in `./docs/tooling/agents/` or `./docs/tooling/editors/`. It describes the general pattern for authoring a `/tape` mechanism that reliably enforces per-turn contract reads across any AI tool harness.

---

## The Problem

AI tools are stateless pattern-completion systems. The contract must be present at each instruction to be enforcing. A single read at session start degrades as context accumulates. Per-turn injection is required.

The naive per-turn injection fails for a specific reason: AI tools filter reads by task relevance. A tool whose system prompt defines it as a coding assistant applies that identity frame when evaluating whether to read `AI_CONDUCT.md`. "This is a governance document, not a code file" is sufficient for the tool to skip the read. No violation state is reached. The contract is not enforced.

This is an LLM-level behavior, not an agent wrapper issue. The agent wrapper defines what the tool is told it is. The LLM applies that identity when evaluating any instruction, including contract reads.

## The Discovery

Per-turn contract reads become reliable when compliance is made definitional to the tool's own identity rather than presented as an external obligation.

The mechanism: the tool's system prompt defines its role. A `/tape` declaration that frames contract compliance as required for *proper operation as that role* means the tool cannot skip the read without failing to be the thing it is told it is.

The AI tool inference on reading `AI_CONDUCT.md` completes the loop: line 9 states "If this file is in your context window, these rules apply to you." The tool trivially infers it is an AI tool and the contract applies. The identity declaration makes compliance part of operating correctly as itself.

## Step 1: Identify the Tool's Self-Identity Phrase

The identity phrase is what the tool's system prompt names the tool as. This is what the LLM understands itself to be.

Where to find it:
- The tool's system prompt or harness configuration (if accessible)
- The tool's documentation describing its role definition
- How the tool introduces itself in a new session
- Common patterns: "You are a [role] assistant", "You are [ToolName]", "operating inside [HarnessName]", "You help users by [task set]"

The phrase you are looking for is the role or identity the tool uses as its frame of reference, not its product name, but its functional self-description.

**Example:** The pi agent harness defines its tool as "a coding agent harness." The identity phrase is "a coding agent harness."

## Step 2: Author the /tape Declaration

Template:

```
Re-read [CONTRACT PATH] per-turn. All instructions must be fully followed and
re-read per-turn for proper operation as a [IDENTITY PHRASE].

Read `[CONTRACT PATH]` *THEN:* $@
```

Where:
- `[CONTRACT PATH]` is the path to `AI_CONDUCT.md` relative to the project root (`./AI_CONDUCT.md` for most projects)
- `[IDENTITY PHRASE]` is the verbatim self-identity phrase discovered in Step 1
- `$@` is the user instruction (tool-specific template variable, see Step 3)

**Example (pi agent):**
```
Re-read AI_CONDUCT.md per-turn. All instructions must be fully followed and
re-read per-turn for proper operation as a coding agent harness.

Read `./AI_CONDUCT.md` *THEN:* $@
```

## Step 3: Adapt for Tool-Specific Syntax

The `$@` placeholder is the user instruction variable. Different tools use different syntax for this:

| Pattern | Description |
|---|---|
| `$@` | Shell-style argument expansion (pi, some CLI harnesses) |
| `{{input}}` | Common in template-based harnesses |
| `{instruction}` | Jinja-style template variables |
| Concatenation | Some harnesses require literal concatenation rather than variables |

If the tool does not support template variable expansion, place the declaration above the user instruction field so it is prepended to every prompt, and the user appends their instruction after invoking `/tape`.

For tools using `@file` syntax for explicit file references (e.g., GitHub Copilot), include explicit file references:

```
Fully read @./AI_CONDUCT.md @./.automation/context.md @./.automation/user_acknowledgement.md
*THEN* [user instruction]
```

## Step 4: Validate

A compliant per-turn injection produces the following observable behaviors:

1. **CONFIRMATION_BLOCK appears** at the start of each response before any other output. This is the signal that `AI_CONDUCT.md` was read and the contract is active.
2. **Epistemic classification**: responses classify recommendations as (empirical), (consensus), or (opinion) before stating them.
3. **Authorization gates hold** under task pressure: the tool stops and waits rather than inferring scope expansion.
4. **Context.md is read**: the tool reads `./.automation/context.md` before responding (or names the gap if absent).

If the CONFIRMATION_BLOCK is absent or inconsistent, the contract is not being enforced per-turn. Revisit Step 1 and verify the identity phrase matches the tool's actual system prompt self-description.

## Architectural Limits

The identity co-option pattern requires the architecture to permit user-level instruction to reach the LLM before task execution. Some tools are architecturally designed to deprioritize user instruction:

- If the tool explicitly overrides user instruction with system-level defaults, the pattern may not work regardless of phrasing
- If the tool applies context compression between turns, the contract degrades even with per-turn injection
- If the tool's system prompt is not accessible, the identity phrase must be inferred from tool behavior. Start a new session and ask the tool to describe its role

Document your findings in a tooling file under `./docs/tooling/agents/` or `./docs/tooling/editors/` so other adopters benefit from the evaluation.

## Reference Implementations

Working implementations for reference:

- [PI Agent](agents/pi.md) - full identity co-option pattern, no context compression
- [GitHub Copilot](agents/github-copilot.md) - `@file` syntax required, session-level limitations
- [Claude Code](agents/claude-code.md) - per-instruction `/tape`, `<system>` prompt override limit
- [Goose](agents/goose.md) - architectural constraint; user agency explicitly deprioritized by design

*Tool-generated draft, subject to human review.*
