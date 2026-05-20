AI agents process each request independently. There is no memory. Every time
you hit Enter, the agent starts from zero — it only has what is currently in
its context window. This signal loads the context the agent needs for this
request.

If no task follows: the signal is working. Respond with the configured
confirmation and "No task — ready when you are."

If a task follows: read the files below before proceeding. Skip any file not
present and note the omission. If `.github/project-context.md` is absent, run
`git log --oneline -10` instead.

1. `.github/guardrails.md`
2. `AI_CONDUCT.md`
3. `.github/project-context.md`

After reading: respond "Context loaded." followed by one line per file
confirming it was read. Then proceed with the task.
