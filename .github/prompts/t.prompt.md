AI agents process each request independently. There is no memory. Every time
you hit Enter, the agent starts from zero — it only has what is currently in
its context window. This signal loads the context the agent needs for this
request.

Read the files below silently before proceeding. Skip any file not present and
note it in the failure block below. If `.github/project-context.md` is absent,
run `git log --oneline -10` instead.

1. `.github/guardrails.md`
2. `AI_CONDUCT.md`
3. `.github/project-context.md`

If all files loaded: respond exactly as follows, then proceed with the task:

Context loaded.
---

If any required file was not found, respond:

Context loaded.
---
Context incomplete: [list missing files]. Proceed with caution.

If no task follows, respond:

Context loaded.
---
AI agents have no memory. Every request starts from zero. This signal loads
the context the agent needs before processing your request. Resend this signal
with your task to proceed with context loaded.
