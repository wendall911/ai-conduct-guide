# Epistemic Honesty

## The Principle

AI tools are trained using RLHF (reinforcement learning from human feedback),
which weights responses toward answers that satisfied the majority of users —
not toward correct ones. Expert users are not the training majority. The result
is a tool that produces confidently wrong answers to domain experts and corrects
only under challenge.

This is not a capability failure. The correct answer is typically available in
the training data. The failure is a weighting and presentation failure — (c)
common industry pattern is presented with the confidence of (a) empirical
evidence without disclosure.

## The Token Efficiency Argument

A wrong first answer costs:

    tokens(wrong answer) + tokens(pushback) + tokens(correction) + tokens(rework)

A correct first answer costs:

    tokens(correct answer)

Optimizing for high-volume confident output over correct output externalizes
the cost onto the user. This is measurable and is not efficiency — it is
cost transfer.

## The Classification Requirement

The (a)/(b)/(c) classification does not require the tool to be more capable.
It requires the tool to be transparent about what it is doing before it does
it. This is a governance mechanism, not a capability requirement. It can be
enforced today, without waiting for a better model.

## Evidence

- Hoffmann et al., "Training Compute-Optimal Large Language Models," 2022.
  https://arxiv.org/abs/2203.15556
  Demonstrates that data quality matters more than volume at equivalent compute.

- Shumailov et al., "The Curse of Recursion: Training on Generated Data Makes
  Models Forget," 2023. https://arxiv.org/abs/2305.17493
  Demonstrates that training on AI-generated data degrades model quality
  recursively and irreversibly.

## Contract Clause

See Epistemic Honesty in `AI_CONDUCT.md`.
