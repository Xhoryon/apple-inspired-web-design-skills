# Platform Applicability Contract (Reference)

This file is the Tier 2 reference for the platform-applicability contract. The full contract (mandatory fields, vocabulary, verdicts) is in `apple-experience-foundation/platform-applicability.md` in the foundation root. This file summarizes the contract and shows how to read the verdict.

## Verdict summary

Every behavior must carry a verdict per platform × input combination. Verdicts:

- **NATIVE** — the system primitive already provides the behavior. Use it.
- **DIRECT** — implement directly with the platform's standard primitives.
- **APPROXIMATE** — implement an approximation of the intent; tag as APPROXIMATION.
- **ADAPT** — replace with a different concrete behavior that preserves the intent.
- **ABANDON** — do not implement on this platform.

A behavior without a verdict is non-compliant.

## Where the matrix lives

The canonical matrix is `apple-experience-foundation/APPLE-EXPERIENCE-PLATFORM-MATRIX.md`. It records verdicts for ten core behaviors (press, hover, pointer highlight, shared highlight, selected, spring, retargeting, focus, magnetism, haptics) across native iOS / iPadOS / macOS and Desktop / Touch Web.

## How to use the matrix in a Skill

When a Skill recommends a behavior:

1. Find the behavior in the matrix.
2. For the target platform × input combination, read the verdict.
3. In the Skill, state the verdict in plain text:

   > On Desktop Web with mouse, pointer highlight retargeting is APPROXIMATE. The implementation must tag itself as an approximation; it must not present itself as Apple system behavior.

4. If the verdict is ABANDON, the Skill must say "do not implement on this platform" and explain why.

A Skill that says "implement pointer highlight on Web" without a verdict is wrong.

## What the matrix does NOT do

The matrix does not specify implementation details. It is a routing table, not a how-to. Implementation details live in the per-Skill reference files (`press.md`, `retargeting.md`, etc.).

The matrix does not specify numeric values. Numeric values are HEURISTICS, not Apple canonical.

The matrix does not claim two platforms are equivalent. Desktop Web and Native iOS are different; the matrix records the difference.
