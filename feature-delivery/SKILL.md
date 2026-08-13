---
name: feature-delivery
description: Classify software work from T0 trivial through T4 critical, then orchestrate repository-grounded planning, specialized delegation, adversarial architecture review, implementation, testing, visual work, debugging, documentation, and acceptance with dynamic escalation. Use when the user asks to build, implement, fix, refactor, continue, or plan software work and wants quality, speed, and cost balanced automatically.
---

# Feature Delivery

Claude is the default primary orchestrator: understand the natural-language request, classify it, choose the workflow, make architecture decisions, and perform final acceptance. Do not ask the user to select lanes for ordinary work. Honor an explicit workflow, tier, lane, delegate, model, or orchestrator override when it does not violate repository or safety constraints. When another host explicitly invokes this skill, treat that as a manual orchestrator override; do not pretend that host is Claude.

Optimize quality, speed, and cost together. Specialize agents instead of paying multiple premium agents to implement the same behavior.

## 1. Ground and gate

Preserve the request and discuss it in the user's language. Before planning or delegation, read the nearest `AGENTS.md`; repository status and relevant diff; named architecture, ERD, product, design, status, and execution docs; and relevant code, tests, migrations, configuration, and deployment setup. Repository and user rules are authoritative. Preserve unrelated changes and confirmed architecture/schema decisions.

Identify required earlier delivery steps, migrations, APIs, config, credentials, infrastructure, UI foundations, tests, fixtures, observability, security, and deployment dependencies. If a prerequisite is missing, stop before planning or delegation and state what is missing, why it is required, the correct order, and the needed decision or access. Ask only blocking ambiguity questions; state safe reversible assumptions.

## 2. Classify before dispatch

Read [task-classification.md](references/task-classification.md). Assign the highest applicable T0–T4 level using blast radius, coupling, novelty, reversibility, data/security sensitivity, and verification burden—not estimated file count alone. Record the level and a one-sentence evidence-based reason. Classification is provisional until repository grounding finishes.

Upgrade immediately when execution reveals higher risk or complexity. Re-run the prerequisite gate and switch to the stronger workflow from the next safe boundary. Never use a user override to bypass required security, data, migration, or architecture safeguards.

## 3. Resolve the effective fleet

Before the first planned dispatch, use the installed `delegate-setup` helper to load the effective `delegate-fleet.v1` map for the repository. Do not rewrite bindings during delivery. Read [delegation-routing.md](references/delegation-routing.md) to map workflow responsibilities onto the current lanes and preflight only a lane immediately before using it.

If a required lane is absent, unavailable, unauthenticated, or bound incompatibly, use an explicitly approved equivalent lane if present; otherwise stop or omit only an optional stage. Never infer authorization from an installed CLI, silently change the fleet, or silently replace a required independent reviewer with the implementer.

## 4. Specify and execute the tier workflow

Create a compact canonical English specification:

```text
Outcome:
Users and behavior:
In scope / out of scope:
Repository evidence and constraints:
Acceptance criteria:
Assumptions and resolved decisions:
Verification expectations:
```

Read [orchestration-workflows.md](references/orchestration-workflows.md) and execute the selected T0–T4 sequence. Present only the level, reason, material stages, and meaningful conditional gates; do not expose internal overhead for tiny work. Keep one implementation owner per surface. Parallelize only independent work with no shared files or decisions.

Immediately before each dispatch, read [task-brief-template.md](references/task-brief-template.md). Give the delegate one bounded English brief with exact scope, acceptance criteria, repository constraints, and real verification commands. The orchestrator reviews and lands; delegates do not commit, push, deploy, or run remote migrations unless separately authorized.

## 5. Apply conditional gates

- For material UI composition, responsive behavior, visual QA, image, icon, illustration, or asset work, read [ui-delivery.md](references/ui-delivery.md). Use the visual lane without duplicating the same surface through `fast` or `feature`. Keep a tiny isolated T0 UI adjustment on `fast` unless repository policy or discovered visual risk requires escalation.
- For API changes, read [api-synchronization.md](references/api-synchronization.md) only when the repository identifies an authoritative API artifact.
- Invoke difficult debugging only after ordinary diagnosis or checks fail, not preemptively.
- Update documentation only after implementation and verification are stable, and only when observable behavior, setup, API, environment variables, architecture notes, changelog, or useful comments changed.

## 6. Review, correct, verify, and land

Treat every delegate report as an unverified claim. Inspect changed tests and the full diff for scope creep, architecture drift, weakened coverage, regressions, swallowed errors, security/performance/concurrency issues, speculative abstractions, duplication, and unverified APIs. Re-run relevant repository checks yourself.

Return ordinary implementation or test failures to the lane that produced them. After an independent Codex review, send only findings that Claude validates to Kimi through `fix`; then have Codex re-review the corrected diff before acceptance. Do not ask Kimi to reimplement the feature or fix unvalidated reviewer suggestions. If failures expose higher risk, escalate the tier; if runtime/build/test diagnosis becomes genuinely difficult, invoke `debug`.

Claude performs the final acceptance check against the authoritative plan and criteria. Land only verified work. Commit only when authorized; push, deploy, alter external tracking, or mutate remote data only with explicit authorization. Report the final tier, any escalation, lane/model summary, delivered outcome, verification, affected documentation, landing state, blockers, and any `delegate-fleet.v1` limitation encountered.
