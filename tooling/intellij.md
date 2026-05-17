# IntelliJ IDEA

## Tool Overview

IntelliJ IDEA hosts GitHub Copilot and other AI plugins. AI behavior is
configured through plugin settings and project-level instruction files.

## Use Case Differences vs VS Code

IntelliJ's native code completion is significantly more capable than VS Code's
out of the box, particularly for JVM languages. AI autocomplete competes with
a stronger baseline, which changes the tradeoff calculation.

For projects where IntelliJ's native completion is sufficient, AI inline
autocomplete adds less marginal value and more interference than in VS Code.
High-value cases remain: repetitive block patterns, boilerplate generation,
unfamiliar APIs.

## Configuration for Conduct Compliance

Add `AI_CONDUCT.md` reference to the Copilot instructions file for the project.
Location varies by plugin version — check plugin settings for the instruction
file path.

## Evaluation

Placeholder — evaluation to be expanded from documented use.
