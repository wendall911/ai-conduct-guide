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
