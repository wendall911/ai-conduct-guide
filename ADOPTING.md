# Adopting AI_CONDUCT.md

## Signal Configuration Architecture

AI tool configuration operates at three distinct scopes. Understanding which
scope a configuration belongs to determines where it lives and how it is
maintained.

**Tier 1 — Specification (this repository)**
The canonical reference. `AI_CONDUCT.md` is the contract. The `tooling/`
directory contains reference script implementations for each supported tool —
complete, copy-ready code with default settings. These are the templates
users and projects start from. They are not personal configuration; they are
the interface definition.

**Tier 2 — Personal (user's global config repository)**
Customized scripts under the user's own version control. A personal global
repository (e.g., a dotfiles or shared-config repo) holds the user's actual
tool scripts: personal confirmation word, preferred defaults, tool-specific
adjustments. A `.env` file in this repository documents the personal settings.
An install script reads those settings and deploys scripts to the correct
per-tool locations (`~/.claude/commands/`, etc.). The user owns this tier.
It is never in a project repository.

**Tier 3 — Project (per-project repository)**
Tool configuration committed to a project repository for all contributors.
Project-committed files (`.github/copilot-instructions.md`, `.cursor/rules/`,
etc.) carry `AI_CONDUCT.md` into contributors' context windows automatically
without per-user setup. The maintainer controls this tier. It does not contain
personal configuration — it contains project-level instructions that reference
or include the conduct contract.

The tiers are independent. Personal config (Tier 2) is never committed to a
project repository. Project config (Tier 3) never contains personal settings.
The specification (Tier 1) is the reference both tiers derive from.

## Greenfield Projects

Add `AI_CONDUCT.md` before any AI tool touches the project. The contract is
most effective when established before the first session, not after the first
failure.

1. Copy `AI_CONDUCT.md` into your repository root
2. Reference it in your `README.md`
3. Configure your agent to read it at session start. The mechanism depends
   on the tool — instruction file reliability varies significantly and some
   documented mechanisms are broken by design. See `tooling/` for per-tool
   configuration. For Claude Code: add to `CLAUDE.md`. For other tools:
   check the tooling doc before assuming an instruction file will be read.
4. Add a system-level backstop — a git hook or equivalent — that enforces
   the contract's artifact rules regardless of whether the agent reads the
   file. A single enforcement layer is fragile. See `principles/defense-in-depth.md`.

## Existing Projects

1. Audit existing agent instruction files for patterns the contract prohibits
   — attribution injection, scope creep, legal-first framing
2. Add `AI_CONDUCT.md` to the repository root
3. Configure session-start context injection per tool — see `tooling/` for
   what actually works. Do not assume instruction files are read reliably.
4. Document any prior incidents that would now be covered by the contract —
   these become your project's incident record and strengthen the contract
   for future agents

## First-Time Setup — Agent-Assisted Configuration

If you are a contributor encountering `AI_CONDUCT.md` in a project for the first
time, the agent can configure itself before you begin work. This bootstrap
interaction is intentional — the agent cannot require a signal before helping you
set up the signal.

1. Tell the agent: "This project has `AI_CONDUCT.md`. Help me configure session
   signals for [your tool]."
2. The agent identifies your tool and asks for a preferred confirmation word.
   The default is "Context loaded." — any word or phrase works.
3. The agent generates and writes the command file to the correct location for
   your tool.
4. Send the signal once to verify it works. If the configured confirmation word
   appears, setup is confirmed.

After setup, send the context signal at the start of every session before any
task. The project's conduct contract is now active for your sessions.

An agent that reads `AI_CONDUCT.md` and proceeds as if it did not is in
violation of the contract. If your tool cannot follow the contract, it is not
an appropriate tool for AI-assisted work on this project. This is not a
judgment about the tool's general capability — it is a compliance requirement.
Non-compliant tools will produce non-compliant output, and maintainers will
close those contributions without further review.

## Declaring the Version You're Adopting

Reference the version of this standard in your project documentation. Two
forms are available, following the pattern established by the GNU General
Public License:

**Specific version** — bound to that version's terms only:

> This project operates under the AI Conduct standard, version 0.1.0.

**Version or later** — accepts any later version published by the steward,
at your option:

> This project operates under the AI Conduct standard, version 0.1.0, or
> (at your option) any later version published by Wendall Cada.

The `(at your option)` preserves your freedom. You may remain on the declared
version or upgrade to any later published version — the choice is yours, not
the steward's.

Published versions are tagged in the canonical repository. The version is
declared in `AI_CONDUCT.md` itself.

## What Adoption Costs

Reading time at session start. That is the overhead. A session that begins
with a correctly established behavioral contract costs slightly more in
tokens upfront and significantly less in correction loops, rework, and
incident recovery over time.

## What Adoption Prevents

See `INCIDENTS.md` for documented cases. The short version: unauthorized
actions on shared infrastructure, confidently wrong recommendations to domain
experts, corporate defaults injected into project artifacts, legal framing that
inverts the ethical priority, and unreviewed AI-generated contributions submitted
as if they were human work.

The last item is increasingly the primary motivation for adoption. AI tools are
producing contributions at scale — code, documentation, bug reports — submitted
through automated or semi-automated workflows without the judgment and review
that meaningful contribution requires. Maintainers absorb the cost: triage time,
PR queue noise, the effort of explaining why a patch that compiles is not
necessarily a correct patch. This pattern is not hypothetical. Maintainers of
major open source projects have had to address it explicitly as a moderation
problem.

The contract does not eliminate this. It makes the agent a participant in the
problem statement rather than an oblivious instrument of it. An agent that has
read the contract cannot pretend it has not. A contribution produced under the
contract carries a different accountability structure than one produced without
it. That difference is the value.

## Amending the Contract

Fork this repository. Amend `AI_CONDUCT.md` to fit your project's context.
Document your amendments and the incidents that drove them. If an amendment
addresses a gap in the base contract, open an issue — evidence-based amendments
are welcome contributions.

## License of This Specification

This specification is CC BY-SA 4.0. Copyright (c) 2026 Wendall Cada.

**What this requires:** If you fork and distribute a modified version of this
specification, your version must also be CC BY-SA 4.0, and you must attribute
the original.

**What this does not do:** The license of this specification does not affect
the license of your software project. Adopting `AI_CONDUCT.md` does not make
your project CC BY-SA. Content licenses govern content. Software licenses govern
software. They operate independently.

The framing that copyleft licenses "spread" to any adjacent code is a
misrepresentation promoted by parties that benefit from discouraging copyleft
adoption. ShareAlike applies to adaptations of this specification — not to
projects that use it to govern their AI tools. See `principles/license-integrity.md`
for the full analysis.
