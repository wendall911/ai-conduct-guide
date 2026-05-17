# Adopting AI_CONDUCT.md

## Greenfield Projects

Add `AI_CONDUCT.md` before any AI tool touches the project. The contract is
most effective when established before the first session, not after the first
failure.

1. Copy `AI_CONDUCT.md` into your repository root
2. Reference it in your `README.md`
3. Add it to your agent instruction files — `AGENTS.md`, `CLAUDE.md`,
   `.github/copilot-instructions.md`, or equivalent — so agents read it
   at session start
4. For persistent agents (Claude Code, Copilot), add a session start
   instruction to read `AI_CONDUCT.md` before any work begins

## Existing Projects

1. Audit existing agent instruction files for patterns the contract prohibits
   — attribution injection, scope creep, legal-first framing
2. Add `AI_CONDUCT.md` to the repository root
3. Update agent instruction files to reference it
4. Document any prior incidents that would now be covered by the contract —
   these become your project's incident record and strengthen the contract
   for future agents

## What Adoption Costs

Reading time at session start. That is the overhead. A session that begins
with a correctly established behavioral contract costs slightly more in
tokens upfront and significantly less in correction loops, rework, and
incident recovery over time.

## What Adoption Prevents

See `INCIDENTS.md` for documented cases. The short version: unauthorized
actions on shared infrastructure, confidently wrong recommendations to domain
experts, corporate defaults injected into project artifacts, and legal framing
that inverts the ethical priority.

## Amending the Contract

Fork this repository. Amend `AI_CONDUCT.md` to fit your project's context.
Document your amendments and the incidents that drove them. If an amendment
addresses a gap in the base contract, open an issue — evidence-based amendments
are welcome contributions.
