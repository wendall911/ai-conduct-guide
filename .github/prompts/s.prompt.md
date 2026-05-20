External state change signal. Include a description of what changed inline.

Something outside this session changed. The description is inline.

CONFIRMATION_BLOCK:
  template: "{WORD}\n{SEP}"
  WORD: "Context loaded."
  SEP:  "***"  # thematic break, not setext — renders <hr> always
  parser: CommonMark likely (unconfirmed for Copilot — community docs only)
  rule: emit byte-exact — deviations are visible UI bugs

If no description follows:
  emit CONFIRMATION_BLOCK
  This signal is for reporting external state changes — a file was updated, a
  decision was made, an external event occurred. When you have a state change
  to report, include the description with this signal.

If a description follows:
  emit CONFIRMATION_BLOCK
  Understood: [one sentence summary of what changed]. Correct?
  Then wait for the user to confirm or correct before proceeding. Do not infer
  beyond the description. Do not proceed past the confirmation without it.
