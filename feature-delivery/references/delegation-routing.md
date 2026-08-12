# Effective fleet routing and preflight

`delegate-fleet.v1` is the source of truth for lane bindings and relay dials. It does not decide task tier or workflow order; this skill does.

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
| Tiny/fast implementation | `fast` | Kimi |
| Main implementation and fixes | `feature` | Kimi |
| Tests and bounded mechanical work | `tests` | OpenCode |
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

- Kimi is the default explorer, implementer, and fixer. Prefer its standalone subscription/configured alias rather than routing Kimi through OpenCode.
- Codex is an independent challenger/reviewer, not the default implementation worker. T3/T4 architecture critique requires verified `gpt-5.6-sol` with high effort; stop for an explicit override if the active binding cannot provide it.
- OpenCode owns tests, bounded low-cost work, difficult GLM debugging, and free-model documentation according to its lane binding.
- GLM-5.2 runs through OpenCode/Z.AI Coding Plan; never treat it as a standalone CLI.
- Antigravity owns visual/UI/assets work when applicable. Prefer existing suitable assets over generating replacements.
- Claude remains outside the fleet as the top-level orchestrator and decision-maker.

## Failure and escalation

Return implementation or review findings to the same responsible implementer. Do not dispatch multiple premium agents to implement the same surface. Run independent Codex critique/review in a separate delegated process/session; the orchestrator's own reasoning cannot satisfy that gate. Invoke the debug lane when a meaningful failure persists after one evidence-based correction and rerun, or standard inspection/output cannot localize a cross-component/runtime cause. A failed or unavailable optional docs/visual lane does not authorize undocumented behavior or unverified visuals; report the limitation and complete only what can be accepted safely.

## `delegate-fleet.v1` limitations

The schema stores named lane → implementer plus optional model/effort/variant/timeout/read-only dials. It cannot encode:

- T0–T4 classification or detection rules;
- workflow sequencing, conditions, escalation, or stop criteria;
- Claude's orchestrator role or final decision authority;
- architecture-debate rounds and objection dispositions;
- independence requirements between implementation and review;
- “docs only if affected” or “debug only if stuck” semantics;
- shared state, outputs, or acceptance criteria between stages.

Those behaviors therefore live in `feature-delivery` instructions and are enforced by the active orchestrator. Fleet validation can prove a lane binding is syntactically valid and trusted; it cannot prove the workflow was followed.
