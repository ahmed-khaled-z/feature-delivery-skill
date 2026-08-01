# Delegation routing and selected-tool preflight

Use this reference only after the prerequisite and ambiguity gates have passed and the current task has been scored.

## Routing precedence

1. Honor an explicit user delegate/model choice first unless it conflicts with non-negotiable repository safety, architecture, security, or cost policy; stop and ask on conflict.
2. Apply repository routing and allowed-model policy.
3. Choose only from approved tools that are available and authenticated in the current environment.

Never silently substitute an unavailable, unauthenticated, disallowed, billed, or weaker tool/model. Record `task | C/I/R | delegate | exact model/alias | reason` before dispatch; the reason identifies the winning routing rule and capability/cost fit.

Load only the selected delegate skill immediately before preflight and dispatch. Preflight only that tool: confirm its executable/version, required authentication/configuration, and selected model or alias. Do not enumerate, probe, or authenticate unrelated delegate CLIs.

## Codex: orchestrator-owned automatic selection

The orchestrator owns Codex model selection. Discover models through the installed Codex CLI/account's currently supported mechanism and verify the chosen model is usable; accepted flags and remembered model names are not a current catalog.

Use a balanced, cost-effective verified model for routine bounded work. Automatically use the strongest capable verified model when any complexity, importance, or risk score is 4 or 5, or when work involves architecture, authentication, payments, security, privacy, concurrency, or destructive migrations. Pass the exact model with `codex-delegate`'s `--model`, and record it and the reason in the queue and dispatch record.

```bash
node "<codex-delegate-skill>/scripts/relay.mjs" --brief brief.txt --model "<verified-model>" --cd /path/to/repo
```

If discovery cannot verify a candidate, choose another approved available delegate/model or ask for a routing decision. Never invent a model or silently fall back to an implicit default.

## OpenCode: human-owned allowlist

The human owns OpenCode eligibility. Before selecting or preflighting OpenCode, find an explicit allowed-model set in the user request or repository policy. `opencode models` is discovery only and never authorization. Within the allowed set, the orchestrator selects the cheapest capable model and records it with the reason. If no allowed set exists, stop and ask; never infer or silently substitute a model.

## Kimi: configured alias policy

When Kimi wins the routing order, use and verify its configured/default alias behavior under its delegate skill. Do not invent, replace, or convert the alias into an unapproved fixed model name. Record the exact configured alias used, or `configured default alias` when that is all the tool exposes, and the reason.
