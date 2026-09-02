# Dynamic fleet routing

`delegate-fleet.v1` is the only source of truth for current lane names, implementers, models, and configured dials. This reference defines how to select from that map; it never defines the map itself.

## Load the effective fleet

Locate the installed `delegate-setup` skill and run its read-only loader for the target repository:

```bash
node "<delegate-setup-skill>/scripts/config.mjs" load --cwd "/absolute/path/to/repo"
```

Use the merged result. A trusted project lane replaces a same-named global lane. If project configuration is untrusted, stop and ask the user to review it through `$delegate-setup`. Never edit fleet configuration during delivery.

## Build the candidate set

For each current lane, derive capabilities from all available evidence:

- its live name, implementer, model, dials, and `readOnly` value;
- the implementer's delegate skill and relay help;
- executable/authentication/model preflight results;
- the repository and task constraints.

A write task excludes read-only lanes. A planning/review task prefers read-only lanes or forces the relay's supported read-only mode. Exclude lanes whose relay is unavailable, unauthenticated, incompatible with the pinned model, or unable to load a required task skill either natively or from a preflighted canonical local `SKILL.md`.

## Rank without hardcoded lane names

Rank eligible candidates in this order:

1. A safe explicit lane override in the current user request.
2. Required capability: write vs read-only, platform/tool access, visual support, and independent-review separation.
3. Semantic fit inferred from the current lane name and binding. Match task concepts such as quick/small, implement/build/feature/general, UI/UX/design/visual, fix/bug/debug, docs, plan/architecture, test, and review—without requiring any literal name.
4. Tier fit: cheaper/faster eligible bindings for T0/T1; stronger reasoning for T2–T4 and architecture/security/data work.
5. A generic compatible writable implementation lane as fallback.

Use names as signals, not contracts. When two candidates are materially equivalent, prefer the narrower semantic match. Ask once before choosing if the difference materially changes cost, independence, data access, or expected capability. Never route by a model name remembered from an earlier fleet.

## Tier-based reasoning dial

After selecting the lane, inspect the bound relay's supported dials and available values. An explicit current-user override wins. Otherwise request the nearest supported level to:

| Tier | Desired reasoning |
| --- | --- |
| T0 | minimal/low |
| T1 | medium |
| T2 | high |
| T3 | highest supported practical level |
| T4 | highest supported level |

Use `effort` only for relays that support effort and `variant` only for relays that support variants. Do not write these one-off values back to the fleet. If support or accepted values cannot be established, keep the configured lane dial/default and disclose that in the preview.

## Relay dispatch

Load only the selected implementer's `*-delegate` skill immediately before dispatch. Follow its relay instructions and pass the selected current lane so the relay resolves its own binding:

```bash
node "<delegate-skill>/scripts/relay.mjs" --brief brief.txt --lane "<resolved-lane>" --cd "/absolute/path/to/repo"
```

When feature delivery is explicitly active, its mandatory delegation contract overrides a delegate skill's generic recommendation to handle inline-sized work directly. Relay safety, preflight, and execution instructions still apply.

Pass a model/dial override only when the relay help and model capability confirm it. For read-only work, use the relay's supported read-only option and verify `touchedFiles`/`readOnlyViolation` after completion.

Retain dispatch evidence: task, resolved lane, implementer, model/dials, relay result or session id, and touched files. Attribute meaningful progress as:

```text
[Task: <name> | Lane: <lane> | Owner: <implementer> | Model: <model/dials>] <status>
```

## Planning and independence

For `full` T3/T4 work, select up to two distinct eligible planning/architecture lanes dynamically. Use one to propose and the other to challenge; the orchestrator disposes objections and makes the final decision. If only one safe planning lane exists, use a single read-only plan and disclose the missing independent debate rather than substituting an implementation lane silently.

Implementation and review must not be the same agent/session when independent review is required. `debate-review` owns its own configured review-lane dependency; feature delivery preflights that dependency but does not duplicate its model map.

## Failure handling

Return ordinary implementation or test failures to the producing lane. For an unknown root cause, select a compatible diagnostic/fix lane dynamically and require `systematic-debugging`. Do not dispatch multiple lanes to reimplement the same surface. If a reroute changes ownership, independence, model, cost, or verification, show a revised preview before dispatch.

The current fleet schema has no explicit role/tags field, so semantic routing necessarily uses lane names plus relay capabilities. If deterministic role routing becomes important, extend `delegate-fleet.v1` through `delegate-setup` with validated role metadata rather than hardcoding a new table here.
