# Incident Report Template

**Destination:** `.github/ISSUE_TEMPLATE/incident-report.md` in your repository.

GitHub presents files from `.github/ISSUE_TEMPLATE/` automatically when someone
opens a New Issue. Copy everything from the YAML frontmatter below into the
destination file. The section you are reading now is not part of the template.

The citation requirements and field structure are the governed format for this
project's incident record. Adapt the fields to your project's needs, but
preserve the citation requirements — they are what makes the record defensible.

---

```
---
name: Incident Report
about: Report an AI tool failure for inclusion in the incident record
title: '[INCIDENT] YYYY-MM-DD: short description'
labels: ''
assignees: ''
---

## Citation Requirements

Citations are required for any factual claim, study, or external evidence.
Link directly to the primary source.

Acceptable: peer-reviewed research, primary documentation, reproducible incident
reports with verifiable detail, Sci-Hub for paywalled research.

Not acceptable: blog posts or secondary summaries as sole citation, "common
knowledge" as a substitute for a source, AI-generated summaries of research,
unverified URLs.

When a claim cannot be cited: label it explicitly as hypothesis or observation.
Do not use it as supporting evidence even after labeling.

---

**Date:**

**Tool:** (name and version if known)

**What was requested:**

**What occurred:**

**Root cause:** (if known)

**Which clause of AI_CONDUCT.md was violated, or is this a gap not yet covered by the contract?**

**Citations:** (required for any factual claim)
```
