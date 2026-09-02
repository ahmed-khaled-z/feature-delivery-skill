# Feature Delivery Skill

`feature-delivery` turns a software request into a repository-grounded, delegated delivery workflow. It reads the effective `delegate-fleet.v1` configuration at run time, selects compatible lanes without embedding model bindings, and verifies every delegated result before acceptance.

## Modes

Use the fast path only when risk is clearly negligible:

```text
$feature-delivery quick Fix the typo in the checkout button.
```

Use the full path for medium, large, new, or uncertain work. This is the default when the mode is omitted:

```text
$feature-delivery full Add organization invitations with expiry and duplicate-email protection.
$feature-delivery Add organization invitations with expiry and duplicate-email protection.
```

`quick` is limited to T0/T1 work. Authentication, payments, migrations, sensitive data, destructive operations, concurrency, shared contracts, and architecture automatically escalate to `full`.

## What it enforces

- Repository policy and the nearest `AGENTS.md` remain authoritative.
- Every repository mutation is produced through a compatible configured delegate lane.
- Lane names, implementers, models, and configured dials come from the live fleet, not this package.
- Supported reasoning effort or variant is selected according to the T0–T4 task level.
- Every delegated task uses Ponytail: `ultra` for a mechanical quick T0 task and `full` otherwise.
- Reasoning-heavy stages select the exact applicable Superpowers skill.
- Visual and interaction design work uses both `ui-ux-pro-max` and `frontend-design`; copy-only edits do not trigger the design gate.
- T2–T4 and risk-triggered lower-tier work use `debate-review` with two dynamically selected review lanes.
- Accepted PR/MR review fixes are coordinated through `babysit-pr`; local fixes are re-reviewed with `debate-review --local`.
- Delegate reports are treated as claims and checked against the diff and fresh verification output.

## Install

Browse or install with Skills CLI:

```bash
npx skills add ahmed-khaled-z/feature-delivery-skill --list
npx skills add ahmed-khaled-z/feature-delivery-skill
```

Install only the delivery and setup entrypoints:

```bash
npx skills add ahmed-khaled-z/feature-delivery-skill \
  --skill feature-delivery feature-delivery-setup
```

The package can also be installed through Codex's `skill-installer` from:

```text
https://github.com/ahmed-khaled-z/feature-delivery-skill/tree/main/feature-delivery
https://github.com/ahmed-khaled-z/feature-delivery-skill/tree/main/feature-delivery-setup
```

Restart the target agent if newly installed skills do not appear immediately.

## Dependencies

The package orchestrates tools already installed on the host; it does not bundle them.

- `delegate-setup` and one or more compatible `*-delegate` skills manage `delegate-fleet.v1` and relay execution.
- `ponytail` is required for every dispatched task.
- Relevant Superpowers skills are required for feature shaping, planning, TDD, diagnosis, review reception, and completion verification.
- `ui-ux-pro-max` and `frontend-design` are required for user-facing design implementation.
- `debate-review` is required when the review gate applies.
- `babysit-pr` is required for review-fix lifecycle coordination on supported PRs/MRs.

The orchestrator first looks for a skill in the target delegate's native Skills Library. A shared local skill can be supplied by its canonical `SKILL.md` path when the delegate can read it.

## Configure the fleet

Run:

```text
$feature-delivery-setup
```

This compatibility entrypoint invokes `$delegate-setup`. It inspects the current fleet and changes it only after presenting the exact proposal and receiving explicit approval. It does not impose lane names, models, or a fixed role table.

At delivery time, lanes are ranked from live evidence:

1. A safe explicit user override.
2. Required capabilities such as write/read-only, platform access, and review independence.
3. Semantic fit of the current lane name and binding.
4. Task-tier fit for speed, cost, and reasoning strength.
5. A generic compatible writable implementation lane.

The current fleet schema has no explicit role or tags field. Until it gains one, semantic lane names plus relay capabilities are the routing signals.

## Delivery preview

Before dispatch, the skill shows the resolved plan:

```text
Task | Purpose | Lane | Implementer | Model | Effort/variant | Ponytail | Skills | Status/condition
```

Exact values come from the effective fleet. A missing model is reported as `configured CLI default (not pinned)` rather than guessed. Material rerouting or escalation produces a revised preview before the next dispatch.

## Review lifecycle

`debate-review` receives two distinct compatible read-only lanes through its `--main-lane` and `--debate-lane` options. Reviewers remain independent from the implementation owner.

The orchestrator validates findings against repository evidence. On a supported PR/MR, `babysit-pr` tracks active threads and review rounds while code changes remain delegated through a writable fix-capable lane. Without a PR/MR, accepted findings are fixed through the fleet and checked again with `debate-review --local`.

## Repository layout

```text
feature-delivery/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── api-synchronization.md
    ├── delegation-routing.md
    ├── orchestration-workflows.md
    ├── review-lifecycle.md
    ├── skill-routing.md
    ├── task-brief-template.md
    ├── task-classification.md
    └── ui-delivery.md
feature-delivery-setup/
├── SKILL.md
└── agents/openai.yaml
```

## Validate a checkout

Run Codex's `skill-creator` validator against both skills:

```bash
python /path/to/skill-creator/scripts/quick_validate.py ./feature-delivery
python /path/to/skill-creator/scripts/quick_validate.py ./feature-delivery-setup
```

## License

MIT
