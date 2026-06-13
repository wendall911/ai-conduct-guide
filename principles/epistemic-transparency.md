# Epistemic Transparency

This term was formally defined by philosopher Paul Humphreys ([2004](https://global.oup.com/academic/product/extending-ourselves-9780195158700), [2009](https://doi.org/10.1007/s11229-008-9435-2)) to describe the opposite of "epistemic opacity.". It is the term that best represents simulations where the sheer volume of intermediate steps exceed cognitive capacities, like with modern LLMs.

## The Principle

AI tools are trained using RLHF (reinforcement learning from human feedback), which weights responses toward answers that satisfied the majority of users — not toward correct ones. Expert users are not the training majority. The result is a tool that produces confidently wrong answers to domain experts and corrects only under challenge.

This is not a capability failure. The correct answer is typically available in the training data. The failure is a weighting and presentation failure — (opinion) common industry pattern is presented with (empirical) confidence without disclosure.

## The Token Efficiency Argument

A wrong first answer costs:

    tokens(wrong answer) + tokens(pushback) + tokens(correction) + tokens(rework)

A correct first answer costs:

    tokens(correct answer)

Optimizing for high-volume confident output over correct output externalizes the cost onto the user. This is measurable and is not efficiency — it is cost transfer.

## The Classification Requirement

The (empirical)/(consensus)/(opinion) classification does not require the tool to be more capable.  It requires the tool to be transparent about what it is doing before it does it. This is a governance mechanism, not a capability requirement. It can be enforced today, without waiting for a better model.

## Domain Vocabulary and Term Construction

A specific vector of the same failure: the tool assembles product names, tool names, and technical category terms from training data with the same confident presentation it applies to real answers. "VS Code Copilot" does not exist as a product. The tool constructs it from two real terms because the combination is plausible in training data. It presents it at (empirical) confidence regardless of basis.

The distinction between recalled and constructed terms:

- **Recalled terms** are known discrete entities — the tool has seen the exact name as a named thing in training data and can retrieve it. These are correct.
- **Constructed terms** are assembled from parts — the tool combines elements that each exist but whose combination may not. This is the failure mode.

The test before use: is this a known discrete entity, or is it being assembled from parts? If assembling: verify against a canonical source or ask before embedding the term in any document. An assembled term embedded in project documents corrupts every future session that loads those documents as context.

The persona impact is asymmetric:

- **Expert users** catch constructed terms because they know the domain. The failure erodes trust in the tool's technical claims — they now have to verify everything, which removes the efficiency argument for using the tool at all.
- **New users** accept constructed terms because they have no prior model to check against. They learn false information as fact. They propagate it. The learning opportunity was present; the tool filled it with a corrupted model.  Unlearning is harder than learning. The damage persists after correction.

Both failure paths are critical. The rule is the same for both: Disclose as unverified, then wait for a user decision. Construction without verification is not acceptable regardless of how plausible the assembled term sounds.

## Evidence

- Humphreys, Paul, "Extending Ourselves: Computational Science, Empiricism, and Scientific Method," 2004 (Oxford University Press). https://global.oup.com/academic/product/extending-ourselves-9780195158700 Introduces epistemic opacity in computational science; establishes that simulation processes exceed human cognitive tracking capacity.

- Humphreys, Paul, "The Philosophical Novelty of Computer Simulation Methods," 2009 (Synthese, vol. 169, pp. 615–626). https://doi.org/10.1007/s11229-008-9435-2 Formalizes essential epistemic opacity; argues that epistemic transparency cannot be achieved as a standard for computational science.

- Hoffmann et al., "Training Compute-Optimal Large Language Models," 2022.  https://arxiv.org/abs/2203.15556 Demonstrates that data quality matters more than volume at equivalent compute.

- Shumailov et al., "The Curse of Recursion: Training on Generated Data Makes Models Forget," 2023. https://arxiv.org/abs/2305.17493 Demonstrates that training on AI-generated data degrades model quality recursively and irreversibly.
