# Feature Delivery Skill

`feature-delivery` turns natural-language software requests into repository-grounded T0–T4 workflows. Claude is the default orchestrator; it classifies the task, chooses the cheapest workflow that preserves the quality floor, coordinates specialized lanes, and performs final acceptance.

## What it enforces

- Repository documentation and `AGENTS.md` remain authoritative.
- Missing prerequisites stop planning and implementation.
- Every repository implementation change, including T0, must be produced through a configured delegate lane; the orchestrator cannot silently implement it directly.
- Architecture and schema decisions are never silently redesigned.
- Tasks stay small, dependency-ordered, and independently verifiable.
- T0/T1 stay fast; T3/T4 receive bounded adversarial architecture review.
- Kimi explores and fixes validated review findings, GLM implements through OpenCode/Z.AI Coding Plan, OpenCode Go writes tests, Codex challenges and reviews, and Antigravity owns visual work.
- Delegate reports are treated as claims and re-verified by the orchestrator.
- UI and API synchronization gates run only when the repository requires them.
- Commits, pushes, deployments, and external mutations follow explicit authorization.

## Install with Skills CLI

Browse the package before installing:

```bash
npx skills add ahmed-khaled-z/feature-delivery-skill --list
```

Install the package with both skills, or select them explicitly:

```bash
npx skills add ahmed-khaled-z/feature-delivery-skill
npx skills add ahmed-khaled-z/feature-delivery-skill --skill feature-delivery feature-delivery-setup
```

Install for a specific agent, or globally:

```bash
npx skills add ahmed-khaled-z/feature-delivery-skill --skill feature-delivery feature-delivery-setup --agent codex
npx skills add ahmed-khaled-z/feature-delivery-skill --skill feature-delivery feature-delivery-setup --agent claude-code
npx skills add ahmed-khaled-z/feature-delivery-skill --global
```

The package contains `feature-delivery` and its configuration command, `feature-delivery-setup`.

## Alternative installation

### Codex skill installer

Ask Codex:

```text
Use $skill-installer to install the feature-delivery skill from:
https://github.com/ahmed-khaled-z/feature-delivery-skill/tree/main/feature-delivery

Then install its setup skill from:
https://github.com/ahmed-khaled-z/feature-delivery-skill/tree/main/feature-delivery-setup
```

Restart Codex after installation if the skill does not appear immediately.

### Manual installation

```bash
git clone https://github.com/ahmed-khaled-z/feature-delivery-skill.git
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
skill_dest="${CODEX_HOME:-$HOME/.codex}/skills/feature-delivery"
setup_dest="${CODEX_HOME:-$HOME/.codex}/skills/feature-delivery-setup"
test ! -e "$skill_dest" || { echo "Destination already exists: $skill_dest"; exit 1; }
test ! -e "$setup_dest" || { echo "Destination already exists: $setup_dest"; exit 1; }
cp -R feature-delivery-skill/feature-delivery "$skill_dest"
cp -R feature-delivery-skill/feature-delivery-setup "$setup_dest"
```

## Delegate prerequisites

The skill coordinates implementation; it does not bundle `delegate-setup`, delegate skills, or their CLIs. The intended fleet uses:

- `delegate-setup` for global/project `delegate-fleet.v1` configuration.
- `codex-delegate` for OpenAI Codex CLI.
- `opencode-delegate` for OpenCode CLI.
- `kimi-delegate` for Kimi Code CLI.
- `agy-delegate` for Antigravity CLI.

Only the selected delegate is loaded and checked for a task.

## Configure delegate tools

Installation does not prompt for delegate choices because the Skills CLI only installs files; it does not run package-specific setup hooks. After installation, run:

```text
$feature-delivery-setup
```

`feature-delivery-setup` is a compatibility entrypoint that invokes `$delegate-setup`. It configures the global or project `delegate-fleet.v1` map only after showing the exact JSON and receiving approval. It does not write `AGENTS.md` or the obsolete `~/.config/feature-delivery/config.json`.

Delegate-specific behavior:

- **Kimi:** `explore` and `fix`; `fix` receives only Claude-validated Codex findings.
- **OpenCode/Z.AI:** GLM-5.2 on `fast`, `feature`, and `debug`.
- **OpenCode Go:** `tests` using the configured test model.
- **Codex:** `architecture` and `review`; T3/T4 architecture critique requires verified GPT-5.6 Sol with high effort or an explicit user override.
- **Antigravity:** `ui` and `assets`.

Manual workflow or lane overrides remain available:

```text
Use $feature-delivery to implement organization invitations, but run the T3 workflow and use my project review lane.
```

Manual overrides cannot bypass repository or safety constraints. The effective fleet remains the lane-binding source of truth; the skill owns classification, sequencing, escalation, and stop conditions.

## Task levels and execution

| Level | Typical work | Execution sequence |
| --- | --- | --- |
| T0 | Typo, spacing, rename, tiny isolated adjustment | Claude → GLM fast → targeted validation |
| T1 | Small isolated bug or straightforward behavior | Claude → GLM → relevant checks/fix → affected docs |
| T2 | Normal contained feature/integration | Kimi explore → Claude plan → GLM implement → OpenCode Go tests → optional Antigravity → Codex review → Kimi fixes validated findings → Codex re-review → Claude acceptance → affected docs |
| T3 | Cross-module, architecture, major data/state flow, substantial regression risk | Full workflow with bounded Claude–Codex architecture debate |
| T4 | Auth, payments, security, migrations, destructive/sensitive/core architecture | Full workflow with maximum verification |

T3/T4 allow one initial Codex critique and one final challenge. Claude labels each objection `ACCEPT`, `PARTIALLY ACCEPT`, or `REJECT`, then makes the final decision; agreement is not required.

The classifier uses the highest applicable level based on blast radius, coupling, novelty, reversibility, data/security sensitivity, production impact, and verification burden. Critical triggers always force T4 even for a small diff.

Dynamic escalation is supported:

- T0/T1 → T2 when shared or coupled behavior appears.
- T2 → T3 when architectural or cross-module complexity appears.
- Any level → T4 when a critical trigger is discovered.
- Any implementation → GLM debug when a failure survives one evidence-based correction/rerun or cannot be localized from standard inspection and output.
- Any level → Antigravity when visual/UI/assets work becomes necessary.

## `delegate-fleet.v1` limitation

The current schema stores only lane names, implementers, and optional dials such as model, effort/variant, timeout, and read-only mode. It cannot encode classification, workflow order, conditional stages, escalation, debate rounds, reviewer independence, Claude's decision authority, acceptance criteria, or technically prevent the active orchestrator from editing files. Those rules live in `feature-delivery` and must be enforced by the active orchestrator.

## Configure repository constraints

Keep source-of-truth, verification, and safety rules in the nearest `AGENTS.md`; keep delegate bindings in `delegate-fleet.v1`. For example:

```md
# Feature delivery policy

- Architecture: docs/architecture.md
- Schema: docs/database-erd.md
- Verification: npm run lint; npm run typecheck; npm test; npm run build
- Do not change confirmed architecture or schema decisions without approval.
- Do not deploy or run remote migrations without explicit authorization.
```

## Usage

Invoke the skill explicitly when you want its complete workflow:

```text
Use $feature-delivery to implement organization invitations.
```

Rough requirements can be written in any language:

```text
استخدم $feature-delivery لتنفيذ دعوات أعضاء الفريق مع انتهاء صلاحية الدعوة ومنع تكرار البريد.
```

Planning-only example:

```text
Use $feature-delivery to analyze and plan the billing history feature. Do not implement yet.
```

The skill will:

1. Read repository policy, architecture, current code, tests, and delivery state.
2. Stop if a required foundation, migration, credential, or earlier delivery step is missing.
3. Ask only blocking questions that cannot be answered from repository evidence.
4. Produce an English implementation specification while discussing it in the user's language.
5. Classify the request as T0–T4 and explain the evidence briefly.
6. Dispatch every repository mutation through its configured lane and retain the lane/implementer plus relay result or session as evidence.
7. Stop instead of implementing directly when a required delegate is unavailable or cannot authenticate.
8. Execute only the stages justified by that level, with dynamic escalation when needed.
9. Synchronize only affected authoritative UI/API/docs artifacts and land only authorized changes.

## Delegation guarantee and troubleshooting

Invoke the skill explicitly in the same request that asks for implementation:

```text
Use $feature-delivery to fix the checkout total calculation.
```

When active, the skill requires delegation for every repository mutation, including one-line T0 changes. The final report should identify the lane and implementer and include the relay result or session evidence. If the required lane is missing, untrusted, unavailable, unauthenticated, or incompatible, the workflow must stop rather than silently edit the repository directly.

Skills are instruction packages, not operating-system hooks, so they cannot technically block file writes when they were not activated or when a host ignores their instructions. Explicit `$feature-delivery` invocation is therefore more reliable than relying on automatic skill selection. For an additional repository-level guard, add this policy to the nearest `AGENTS.md`:

```md
## Mandatory delegated implementation

When `$feature-delivery` is active, do not edit repository implementation artifacts directly. Every repository mutation must be produced by a configured delegate lane. Stop and report the blocker if dispatch cannot run.
```

## Repository layout

```text
feature-delivery/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── api-synchronization.md
    ├── delegation-routing.md
    ├── orchestration-workflows.md
    ├── task-classification.md
    ├── task-brief-template.md
    └── ui-delivery.md
feature-delivery-setup/
├── SKILL.md
└── agents/
    └── openai.yaml
```

## Validate a local checkout

Run Codex's `skill-creator` validator against the skill directory:

```bash
python /path/to/skill-creator/scripts/quick_validate.py ./feature-delivery
python /path/to/skill-creator/scripts/quick_validate.py ./feature-delivery-setup
```

## License

MIT
