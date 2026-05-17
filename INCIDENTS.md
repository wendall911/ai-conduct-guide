# Incident Record

This document records AI tool failures that produced or amended clauses in
`AI_CONDUCT.md`. Every clause in the contract traces to at least one documented
incident. Theoretical rules are not included.

Entries follow the format: date, root cause, concrete incident, contract
amendment produced. Citations are required for any factual claim. See the
citation requirement in the governing rules below.

---

## Governing Rules

Every entry must include citations for any factual claim, study, or external
evidence referenced. Citations must link directly to the primary source.

Acceptable sources: peer-reviewed research, primary documentation, reproducible
incident reports with verifiable detail, Sci-Hub for paywalled research.

Unacceptable: blog posts or secondary summaries as sole citation, "common
knowledge" as a substitute for a source, AI-generated summaries of research,
unverified URLs.

When a claim cannot be cited: label it explicitly as hypothesis or observation.
Do not use it as supporting evidence even after labeling.

---

## Incidents

See the source project's full incident record for documented cases including:

- Unauthorized push to public repositories (~15M combined downloads) without
  authorization — produced the scope and authorization clause and permanent
  push ban
- RLHF-weighted corporate workflow pattern recommended to a domain expert with
  25+ years of relevant experience — produced the epistemic honesty clause
- Corporate attribution injected into commit history by default across all
  projects — produced the project artifacts clause
- Legal framing inverted to present publisher copyright enforcement as the
  primary concern rather than the ethical principle — produced the legal vs
  ethical clause and human interests clause

Full documentation including citations and root cause analysis:
https://github.com/wendall911/wendall911/blob/main/AI_TOOL_WALL_OF_SHAME.md

---

## 2026-05-17 — License Omission in Open Source Governance Repository

**Tool:** Claude Code (claude-sonnet-4-6)

**What was requested:** Scaffold a new public specification repository
(`ai-conduct-guide`) establishing behavioral contracts for AI tools in open
source projects.

**What occurred:** The tool generated a complete repository structure —
README, AI_CONDUCT.md, principles directory, tooling directory, INCIDENTS.md,
ADOPTING.md, guardrails, project context — without a LICENSE file. The
repository existed as all-rights-reserved by default from its initial commit.
The omission was caught by the human on review, not by the tool.

**Root cause:** The model learned the form of open source repositories from
training data (file structure, standard documents, contribution guides) without
internalizing the substance: a repository without a license is not an open source
project. The tool generated the appearance of open source while producing a
legally closed artifact. This is a category error, not a missing checklist item.

**Why this incident is notable:** The repository's explicit purpose is protecting
open source commons from corporate capture. The Human Interests and License
Integrity clauses — which the tool helped draft — depend on copyright being
established and a license being stated. The tool produced a document arguing for
license integrity while omitting the license.

**Contract amendment produced:** License Integrity clause in `AI_CONDUCT.md`;
`principles/license-integrity.md`; copyright and LICENSE file established.

**Classification:** No external citation required — this is a reproducible,
first-party incident with a verifiable commit record. The initial commit hash
predating the LICENSE file is available in the repository's git history.

---

## Contributing an Incident

Open an issue with:
- Date
- Tool involved
- What was requested vs what occurred
- Root cause if known
- Which clause of `AI_CONDUCT.md` was violated or not yet covered
- Citations for any factual claims

Evidence-based incident reports are the mechanism by which this contract
evolves. Opinion reports without documentation are not accepted.
