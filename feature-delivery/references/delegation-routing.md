# Effective fleet routing and preflight

`delegate-fleet.v1` is the source of truth for lane bindings and relay dials. It does not decide task tier or workflow order; this skill does.

## Mandatory dispatch contract

Any stage that mutates repository implementation artifacts must run through the selected effective fleet lane. The orchestrator may inspect and verify the result but must not author the mutation. This contract applies to all tiers and to code, tests, migrations, configuration, documentation, and assets.

Record the selected lane and implementer together with the relay result or session identifier. Without that evidence, treat the mutation as not delegated and do not accept it. If a required dispatch fails preflight or execution, report the blocker; do not use the orchestrator as an implicit fallback. Only a current explicit user request for direct or non-delegated implementation suspends this contract.

## Load without mutation

Locate the installed `delegate-setup` skill and run its read-only loader for the target repository:

```bash
node "<delegate-setup-skill>/scripts/config.mjs" load --cwd "/path/to/repo"
```

Use the effective merged lanes. Project lanes replace same-named global lanes. If project config exists but is untrusted, do not dispatch it; ask the user to review and approve it through `$delegate-setup`. Never edit fleet files during feature delivery.

## Expected responsibility lanes

| Responsibility | Preferred lane | Expected implementer |
| --- | --- | --- |
| Repository evidence | `explore` | Kimi |
| Tiny/fast implementation | `fast` | OpenCode + Z.AI GLM-5.2 |
| Main nonvisual implementation | `feature` | OpenCode + Z.AI GLM-5.2 |
| Tests | `tests` | OpenCode Go configured test model |
| Validated Codex review fixes | `fix` | Kimi standalone |
| Difficult debugging | `debug` | OpenCode with Z.AI GLM-5.2 model |
| UI/responsive/visual QA | `ui` | Antigravity (`agy`) |
| Images/icons/illustrations/assets | `assets` | Antigravity (`agy`) |
| Independent implementation review | `review` | Codex |
| Adversarial architecture critique | `architecture` | Codex GPT-5.6 Sol, high effort |
| Post-stability documentation | `docs` | OpenCode capable free model |

These are responsibility contracts, not permission to rewrite the fleet. Manual user lane/model overrides win when safe. If a preferred lane is absent, inspect the effective map for an explicit compatible equivalent. Ask before substituting when independence, cost, or capability would change.

## Dispatch rules

Load only the selected `*-delegate` skill immediately before use. Preflight that lane's executable, authentication, model, and dials; do not probe unrelated tools. Dispatch with the configured lane so the relay resolves and validates its own binding:

```bash
node "<delegate-skill>/scripts/relay.mjs" --brief brief.txt --lane "<lane>" --cd "/path/to/repo"
```

Do not invent model identifiers or bypass an unavailable/untrusted lane with an implicit CLI default. Preserve these ownership rules:

- Kimi explores and fixes only Claude-validated Codex review findings through `fix`. Prefer its standalone subscription/configured alias; do not use Kimi for the initial implementation or tests.
- Codex is an independent challenger/reviewer, not the default implementation worker. T3/T4 architecture critique requires verified `gpt-5.6-sol` with high effort; stop for an explicit override if the active binding cannot provide it.
- GLM-5.2 through OpenCode/Z.AI Coding Plan owns `fast`, `feature`, and difficult `debug` work; never treat GLM as a standalone CLI.
- OpenCode Go owns `tests`, and the configured free OpenCode model owns documentation.
- Antigravity owns visual/UI/assets work when applicable. Prefer existing suitable assets over generating replacements.
- Claude remains outside the fleet as the top-level orchestrator and decision-maker.

## Failure and escalation

Return ordinary implementation/test failures to their producing lane. Send only Claude-validated Codex findings to Kimi through `fix`, then return the corrected diff to Codex for re-review. Do not dispatch multiple agents to reimplement the same surface. Run independent Codex critique/review in a separate delegated process/session; the orchestrator's own reasoning cannot satisfy that gate. Invoke `debug` when a meaningful failure persists after one evidence-based correction and rerun, or standard inspection/output cannot localize a cross-component/runtime cause. Docs and visual stages are conditional when selecting a workflow, but once repository evidence makes one necessary its delegate stage is required. A failed or unavailable docs/visual lane never authorizes the orchestrator to edit that surface directly; report the limitation and complete only what can be accepted safely.

## `delegate-fleet.v1` limitations

The schema stores named lane → implementer plus optional model/effort/variant/timeout/read-only dials. It cannot encode:

- T0–T4 classification or detection rules;
- workflow sequencing, conditions, escalation, or stop criteria;
- Claude's orchestrator role or final decision authority;
- architecture-debate rounds and objection dispositions;
- independence requirements between implementation and review;
- “docs only if affected” or “debug only if stuck” semantics;
- shared state, outputs, or acceptance criteria between stages.
- technical prevention of direct edits by the active orchestrator.

Those behaviors therefore live in `feature-delivery` instructions and are enforced by the active orchestrator. Fleet validation can prove a lane binding is syntactically valid and trusted; it cannot prove the workflow was followed. For reliable activation, invoke `$feature-delivery` explicitly and retain the dispatch evidence required above.
