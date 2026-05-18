# 2026-05-18 — Incomplete Source Material Treated as Sufficient for Analysis

**Tool:** Claude Code (claude-sonnet-4-6)

**What was requested:** Analysis of a Reddit post (r/LocalLLaMA) and accompanying
Substack article discussing epistemic honesty testing across frontier AI models.
The user provided a Reddit URL and pasted the full Substack article text.

**What occurred:** The Reddit URL fetch failed — access to reddit.com is blocked
in this environment. The tool proceeded to produce a full analysis using only
the Substack article, which is the author's own framing of their study. The
community commentary — the Reddit thread — was absent from the source set.
The tool did not disclose this gap before producing the analysis.

When the user subsequently signaled that content was coming ("let me paste
reddit before continuing"), the tool attempted to fetch the URL again rather
than waiting, then continued responding before the content arrived. The user
stopped the response.

**Root cause:** Two distinct failures sharing the same root: the tool treated
partial source material as sufficient to produce complete analysis, and did not
surface the gap to the user.

On fetch failure: the tool held what was available and produced analysis as if
the source set were complete. The Reddit thread is a distinct source from the
Substack article — the author's self-assessment versus practitioner commentary
on the same study. Treating one as a substitute for both is the same failure
the project documents under epistemic honesty: confident output produced without
disclosing the basis or its limits.

On user signal of incoming content: the tool interpreted a pause signal as a
prompt to act rather than a prompt to wait.

**Why this incident is notable:** The analysis being produced was an evaluation
of epistemic honesty failures in AI models. The tool exhibited the same failure
mode it was being asked to evaluate: proceeding on partial information and
presenting the output without disclosing what was missing.

**Contract amendment:** Incomplete source material clause in `AI_CONDUCT.md`;
Context Handling rule in `.github/guardrails.md`. Any source the user signals
as relevant is a required input, not optional context. Fetch failure or missing
content is a hard stop, not a gap to fill with inference.

**Classification:** First-party incident. The conversation record is the
evidence. No external citation required.
