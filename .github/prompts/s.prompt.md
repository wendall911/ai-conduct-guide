External state change signal. Include a description of what changed inline.

Something outside this session changed. The description is inline.

If no description follows, output these two lines with no blank line between them, then continue:

Context loaded.
---
This signal is for reporting external state changes — a file was updated, a
decision was made, an external event occurred. When you have a state change
to report, include the description with this signal.

If a description follows, output these two lines with no blank line between them, then continue:

Context loaded.
---
Understood: [one sentence summary of what changed]. Correct?

Then wait for the user to confirm or correct before proceeding. Do not infer
beyond the description. Do not proceed past the confirmation without it.
