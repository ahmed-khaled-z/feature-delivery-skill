# Delegation routing and selected-tool preflight

Use this reference only after the prerequisite and ambiguity gates have passed and the current task has been scored.

## First-use delegate policy gate

Resolve delegate authorization before feature analysis for any implementation or delegation request:

1. Use delegates explicitly enabled by the current user request.
2. Otherwise use delegates explicitly enabled by the nearest `AGENTS.md`.
3. Otherwise use `~/.config/feature-delivery/config.json` when it exists, has `version: 1`, and explicitly enables at least one delegate.
4. If no valid policy exists, stop and ask the user to run `$feature-delivery-setup`. Do not silently switch to direct implementation.

Installed skills, installed CLIs, and conditional guidance that merely mentions a delegate are not authorization. Planning-only requests can complete analysis, prerequisite checks, specification, and a delegate-unassigned plan without a policy.

When OpenCode is selected and neither the request nor repository policy supplies its allowed-model set, ask for the allowlist before selecting or preflighting OpenCode. This can be a second concise question after delegate selection when necessary. Codex does not require a model allowlist question; Kimi retains its configured alias.

## Routing precedence

1. Honor an explicit user delegate/model choice first unless it conflicts with non-negotiable repository safety, architecture, security, or cost policy; stop and ask on conflict.
2. Apply repository routing and allowed-model policy.
3. Choose only from user- or repository-approved tools that are available and authenticated in the current environment.

Never silently substitute an unavailable, unauthenticated, disallowed, billed, or weaker tool/model. Record `task | C/I/R | delegate | exact model/alias | model class | reason` before dispatch; the reason identifies the winning routing rule and capability/cost fit.

Load only the selected delegate skill immediately before preflight and dispatch. Preflight only that tool: confirm its executable/version, required authentication/configuration, and selected model or alias. Do not enumerate, probe, or authenticate unrelated delegate CLIs.

## Mandatory cost-balance portfolio

Classify Kimi K3, ChatGPT Sol, and GLM 5.2, including versioned names and aliases belonging to those families, as `premium`. Classify every other approved and verified model as `non-premium`. Match family names case-insensitively and record the resolved class; do not relabel an alias to bypass this rule.

For `N` dispatched implementation tasks, assign at least `ceil(N / 2)` tasks to non-premium models. Count bounded implementation tasks that produce code, tests, migrations, configuration, or repository artifacts. Exclude orchestrator-only analysis, review, verification, blocked tasks, and cancelled tasks. Recompute the requirement whenever tasks are added, split, cancelled, blocked, or reassigned, and report both planned and completed ratios.

Use the cheapest approved, verified model that is capable of each bounded task. Prefer non-premium models for mechanical and low-to-moderate work whose complexity, importance, and risk scores are all 3 or lower and which does not involve architecture, authentication, payments, security, privacy, concurrency, or destructive migrations. Preserve task coherence; never create trivial or artificial tasks to manipulate the ratio.

The ratio is a cost constraint, not permission to weaken the quality floor. Keep high-capability routing for work that needs it. If approved and verified non-premium models cannot safely complete enough tasks, stop before dispatch and ask the user to change scope, enable capable non-premium models, or explicitly revise the portfolio requirement. An unavailable model, failed task, or unverified result does not satisfy the quota.

## Codex: orchestrator-owned automatic selection

The orchestrator owns Codex model selection. Discover models through the installed Codex CLI/account's currently supported mechanism and verify the chosen model is usable; accepted flags and remembered model names are not a current catalog.

Use a balanced, cost-effective verified model for routine bounded work and apply the portfolio rule above. Automatically use the strongest capable verified model when any complexity, importance, or risk score is 4 or 5, or when work involves architecture, authentication, payments, security, privacy, concurrency, or destructive migrations. Pass the exact model with `codex-delegate`'s `--model`, and record it, its model class, and the reason in the queue and dispatch record.

```bash
node "<codex-delegate-skill>/scripts/relay.mjs" --brief brief.txt --model "<verified-model>" --cd /path/to/repo
```

If discovery cannot verify a candidate, choose another approved available delegate/model or ask for a routing decision. Never invent a model or silently fall back to an implicit default.

## OpenCode: human-owned allowlist

The human owns OpenCode eligibility. Before selecting or preflighting OpenCode, find an explicit allowed-model set in the resolved request, repository, or global policy. `opencode models` is discovery only and never authorization. Within the allowed set, the orchestrator selects the cheapest capable model and records it with the reason. If no allowed set exists, stop and ask; never infer or silently substitute a model.

## Kimi: configured alias policy

When Kimi wins the routing order, use and verify its configured/default alias behavior under its delegate skill. Do not invent, replace, or convert the alias into an unapproved fixed model name. Record the exact configured alias used, or `configured default alias` when that is all the tool exposes, and the reason.
