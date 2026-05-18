# 2026-05-17 — License Omission in Open Source Governance Repository

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
