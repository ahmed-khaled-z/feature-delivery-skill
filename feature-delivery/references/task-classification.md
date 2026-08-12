# T0–T4 task classification

Use the highest triggered level. File count is supporting evidence, not a classifier by itself.

| Level | Detection signals | Priority |
| --- | --- | --- |
| T0 — Trivial | Mechanical, isolated, obvious change such as typo, copy, spacing, rename, tiny UI adjustment, or very small config edit; no behavior ambiguity, shared contract, data/schema, security, concurrency, or meaningful regression surface | Maximum speed |
| T1 — Simple | Localized bug, validation, mapping, UI behavior, or straightforward modification with a known pattern and narrow blast radius; targeted checks can establish correctness | Speed with targeted validation |
| T2 — Standard | Normal feature or contained business logic/integration spanning related files; needs planning, implementation tests, and independent review but not architecture debate | Balanced quality, speed, and cost |
| T3 — Complex | Cross-module behavior, significant state/data-flow change, major refactor, architectural decision, unfamiliar integration, concurrency, or substantial regression surface | Quality and architectural correctness |
| T4 — Critical | Authentication/authorization, payments/refunds, security-sensitive behavior, database migrations, destructive or irreversible operations, sensitive data flows, core architecture, or failure with serious production impact | Maximum correctness and independent verification |

## Decision rules

1. Apply T4 hard triggers first. A tiny code diff does not lower authentication, payment, migration, destructive-operation, or sensitive-data work below T4.
2. Otherwise choose T3 when correctness depends on cross-module or architectural reasoning rather than a contained implementation plan.
3. Otherwise choose T2 when ordinary feature work benefits from separate exploration/planning, tests, and review.
4. Otherwise choose T1 when the behavior changes but remains isolated and directly testable.
5. Use T0 only when the change is mechanical and its risk is genuinely negligible.

Classify from request evidence, then confirm after grounding. Upgrade at any time. Downgrade only before the first dispatch when repository evidence disproves the original risk signal, and record why. A manual user override can select a stronger workflow or preferred lane; it cannot suppress mandatory safety gates.

Examples are illustrative, not exhaustive. Consider blast radius, coupling, novelty, reversibility, production exposure, data/security sensitivity, and the cost of a false pass.
