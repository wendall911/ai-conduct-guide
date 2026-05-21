# Incident: Session Continuity Failure Silently Invalidates Contract Enforcement

**Date:** 2026-05-18
**Tool:** All evaluated AI coding tools (Claude Code, Cursor, Copilot, Windsurf)
**Severity:** High — contract enforcement degrades silently mid-session with no notification to the user

## What Happened

During an active session, context compression occurred without notification. The agent continued operating with degraded context, and conduct rules that were established at session start were no longer reliably enforced. The user had no way to know the enforcement layer had degraded. Unauthorized commits were made. The user only discovered the failure through its consequences.

## Root Cause

AI_CONDUCT.md is read once at session start and held in context. It is not re-read. As the session grows, context compression occurs silently. The contract may be partially or fully evicted from active context. The agent continues to respond with confidence, giving no indication that the rules it was operating under are no longer present.

No evaluated tool provides notification when context is lost or compressed. This is a documented gap with open feature requests and no committed resolution timeline.

## The Defense in Depth Failure

The Defense in Depth principle requires multiple independent enforcement layers. Session continuity failure is a third documented case where single-layer enforcement fails:

- Layer 1 failure: tool configuration (attribution injection — documented)
- Layer 2 failure: instruction file mechanisms broken across tools (documented)
- Layer 3 failure: session context loss silently removes the contract from enforcement

A contract read once at session start and not re-anchored is a single enforcement layer. It fails when the session degrades.

## Scope

This affects all tools evaluated in this project. Claude Code is the most reliable tool found, but it is not immune — context compression is documented behavior. No tool notifies the user when compression crosses the threshold where the contract is at risk.

The user cannot monitor this gap because they do not know when context is lost. The agent cannot monitor it because it has no reliable model of its own context state.

## What This Means for AI_CONDUCT.md Adoption

A project that adopts AI_CONDUCT.md and relies solely on session-start injection has one enforcement layer. That layer degrades silently over time. Human oversight is not optional — it is structural. The contract gives oversight structure; it does not replace oversight.

## Resolution

No tool-level resolution exists as of 2026-05-18. The community has converged on handoff documents as the closest available mitigation. See discussion in project for proposed HANDOFF.md approach.

This incident warrants a new section in `principles/defense-in-depth.md` and acknowledgment in tooling docs that session continuity is an unsolved problem across all evaluated tools.

## Discovered Gap: Handoff Document Loading Is Not Reliable (2026-05-20)

The handoff document approach noted above has a documented failure mode.

**What happened:** The same migration task was attempted in two successive sessions with identical starting conditions (`/t let's continue with the migration`). In the first session, the agent loaded the tape's required files, then made a silent judgment call about what additional context was relevant. The handoff document (MIGRATION.md) was not fully read before the agent began acting. The session failed — wrong output, rework, hallucinated reconstruction of prior context.

In the second session, the agent read the handoff document fully and all files the task step referenced before proposing any action. The task completed without rework.

**The gap:** The agent decides what context to load. That decision is not surfaced to the user. A handoff document present in the project is not automatically loaded — the agent must choose to read it. When it does not (or reads it partially), the user has no indication. The agent proceeds on insufficient context with the same confidence as if it had loaded everything. The handoff document mechanism fails silently.

**Demonstrated mitigation:** Making the context-loading decision explicit and visible before action begins. In the successful session, the agent surfaced which files it intended to read, read them all before proposing any action, and confirmed with the user. The task-relevant context determined which files to load — not a tape rule or memory entry.

**Implication for the resolution:** A handoff document is a necessary but not sufficient mitigation. It provides no continuity if the agent silently decides not to read it. The mitigation requires both: a handoff document AND a mechanism that makes context-loading decisions visible before action begins. A handoff document the agent reads partially is equivalent to one that does not exist.
