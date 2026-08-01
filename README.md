# Feature Delivery Skill

`feature-delivery` turns rough software feature requests into repository-grounded, prerequisite-first delivery work. It resolves blocking ambiguity, creates a canonical specification, decomposes the feature into bounded tasks, routes each task through an approved delegate/model, and independently reviews and verifies the result before landing.

## What it enforces

- Repository documentation and `AGENTS.md` remain authoritative.
- Missing prerequisites stop planning and implementation.
- Architecture and schema decisions are never silently redesigned.
- Tasks stay small, dependency-ordered, and independently verifiable.
- Delegate reports are treated as claims and re-verified by the orchestrator.
- UI and API synchronization gates run only when the repository requires them.
- Commits, pushes, deployments, and external mutations follow explicit authorization.

## Install

### With Codex skill installer

Ask Codex:

```text
Use $skill-installer to install the feature-delivery skill from:
https://github.com/ahmed-khaled-z/feature-delivery-skill/tree/main/feature-delivery
```

Restart Codex after installation if the skill does not appear immediately.

### Manual installation

```bash
git clone https://github.com/ahmed-khaled-z/feature-delivery-skill.git
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
skill_dest="${CODEX_HOME:-$HOME/.codex}/skills/feature-delivery"
test ! -e "$skill_dest" || { echo "Destination already exists: $skill_dest"; exit 1; }
cp -R feature-delivery-skill/feature-delivery "$skill_dest"
```

## Delegate prerequisites

The skill coordinates implementation; it does not bundle delegate CLIs. Install and authenticate at least one compatible delegate skill and its CLI:

- `codex-delegate` for OpenAI Codex CLI.
- `opencode-delegate` for OpenCode CLI.
- `kimi-delegate` for Kimi Code CLI.

Only the selected delegate is loaded and checked for a task.

## Configure a project

Put project-specific rules in the repository's nearest `AGENTS.md`, not inside this global skill. A minimal policy can look like:

```md
# Feature delivery policy

## Source of truth

- Architecture: docs/architecture.md
- Schema: docs/database-erd.md

## Verification

- npm run lint
- npm run typecheck
- npm test
- npm run build

## Delegation

- Codex: enabled; model selection is automatic per task.
- OpenCode: enabled only with these user-approved models:
  - provider/model-a
  - provider/model-b
- Kimi: use the configured/default alias.

## Landing

- Do not change confirmed architecture or schema decisions without approval.
- Do not deploy or run remote migrations without explicit authorization.
```

For OpenCode, the list is an allowlist: `opencode models` discovers entries but does not authorize spending. The orchestrator chooses the cheapest capable model only from the human-approved set.

For Codex, the orchestrator selects a verified available model from the current Codex environment. Routine bounded work receives a balanced model; tasks with a complexity, importance, or risk score of 4–5—or work involving architecture, authentication, payments, security, privacy, concurrency, or destructive migrations—receive the strongest capable model.

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
5. Present a task queue containing `task | C/I/R | delegate | exact model/alias | reason`.
6. Dispatch one bounded task at a time, review its diff, request corrections, and run the real repository gates.
7. Synchronize only authoritative UI/API/docs artifacts and land only authorized changes.

## Repository layout

```text
feature-delivery/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── api-synchronization.md
    ├── delegation-routing.md
    ├── task-brief-template.md
    └── ui-delivery.md
```

## Validate a local checkout

Run Codex's `skill-creator` validator against the skill directory:

```bash
python /path/to/skill-creator/scripts/quick_validate.py ./feature-delivery
```

## License

MIT
