# Tiered orchestration workflows

Claude owns classification, routing, decisions, acceptance criteria, and final verification. If the user explicitly overrides the orchestrator, references to Claude in this document mean that active orchestrator for the run. Keep stages conditional and avoid orchestration work that cannot improve the result.

## T0 — Ultra fast

`Claude route → GLM implementation through fast → targeted validation → done`

- Use the `fast` lane.
- Skip exploration reports, architecture debate, Codex review, GLM debugging, and OpenCode tests unless repository evidence makes one genuinely necessary.
- The `fast` implementer may update the minimal directly affected test at this level. Targeted validation means diff inspection plus the narrowest existing relevant check; do not add broad test infrastructure for a literal mechanical change.
- Update documentation only when documented behavior changed.

## T1 — Fast

`Claude route → GLM implementation through fast/feature → relevant checks/tests → same lane fixes failures → docs if affected → Claude acceptance → done`

- Use `fast` or `feature` according to the effective fleet.
- The implementation lane may update directly affected localized tests at this level.
- Keep validation targeted. Do not invoke expensive reasoning unless findings force escalation.

## T2 — Standard

`Kimi explore → Claude plan → GLM implement → OpenCode Go tests → Antigravity visual/UI/assets work when applicable → Codex independent review → Kimi fixes validated findings → Codex re-review → Claude acceptance → docs if affected`

- Exploration is evidence gathering, not architecture design.
- Use `explore`, `feature`, `tests`, optional `ui`/`assets`, `review`, `fix` only when review findings are validated, and optional `docs`.
- GLM through OpenCode/Z.AI Coding Plan owns main implementation. OpenCode Go owns new or changed tests at T2+; Kimi must not duplicate either task.
- Do not run the Claude–Codex architecture debate for ordinary T2 work.
- If Antigravity owns a UI/visual surface, GLM must not independently implement the same surface. Re-run affected checks after visual work.
- Claude's acceptance at this stage establishes implementation stability. Run one final landing check after any affected documentation is updated.

## T3/T4 — Full

1. Kimi explores through the `explore` lane and produces repository evidence only.
2. Claude writes the initial architecture proposal.
3. Codex uses the `architecture` lane as an adversarial reviewer with verified `gpt-5.6-sol`, effort `high`. If that required binding is unavailable, stop for an explicit override instead of silently weakening the debate.
4. Claude labels every objection `ACCEPT`, `PARTIALLY ACCEPT`, or `REJECT` with repository evidence and rationale.
5. Codex receives one final challenge round.
6. Claude makes the authoritative decision and writes the implementation plan and acceptance criteria.
7. GLM-5.2 performs the main nonvisual implementation through OpenCode/Z.AI Coding Plan on `feature`.
8. OpenCode Go owns tests through `tests`.
9. Antigravity owns applicable UI, responsive, visual, image, icon, illustration, and asset surfaces through `ui` or `assets`.
10. Run repository build, analysis, and tests.
11. Invoke `debug` only for difficult unresolved build, test, runtime, log, or root-cause failures. The expected binding is OpenCode with Z.AI Coding Plan `zai-coding-plan/glm-5.2`; GLM is not a standalone CLI.
12. Codex performs an independent final implementation review through `review`.
13. Claude validates each finding; Kimi fixes only validated findings through `fix`.
14. Codex re-reviews the corrected diff through the same independent `review` session or an explicitly tracked fresh review.
15. Claude verifies every acceptance criterion.
16. If documentation is affected, use `docs` after stability; the expected low-cost binding is `opencode/deepseek-v4-flash-free`.
17. Run a final landing check after documentation. The implementation acceptance decision remains Claude's.

The authoritative plan must assign non-overlapping surfaces such as backend behavior, migration tooling, deployment configuration, tests, and UI. GLM owns main nonvisual implementation, OpenCode Go owns T2+ test files, Antigravity owns assigned visual surfaces, and Kimi's `fix` scope is limited to Claude-validated Codex findings.

An independent Codex critique or review must run in a separate delegated process/session with a fresh bounded brief and repository evidence. The active orchestrator's own reasoning never counts as the required independent Codex pass, even when Codex is the manually overridden orchestrator.

### Bounded architecture debate

Kimi's exploration report must identify relevant modules/files/symbols, current architecture, similar implementations, state/data flow, APIs, tests, platform/flavor behavior, regression areas, and constraints from code. Kimi must not design the solution in this phase.

Codex must try to disprove Claude's proposal: challenge assumptions, architecture, accidental complexity, simpler alternatives, regressions, state/concurrency, security, performance, edge cases, and tests. Ground disagreements in repository evidence whenever possible.

Allow at most two Codex challenge rounds: the initial critique and one final challenge after Claude's evaluation. Stop after Claude's final decision; agreement is not required.

## Dynamic escalation

- T0/T1 → T2 when the change reaches shared behavior, multiple coupled surfaces, or needs independent review.
- T2 → T3 when architectural, cross-module, state/data-flow, concurrency, or substantial regression complexity appears.
- Any level → T4 when a critical trigger is discovered.
- Any implementation → `debug` when the same meaningful build/test/runtime failure persists after one evidence-based correction and rerun, or when standard code inspection and command output cannot localize a cross-component/runtime cause. A first ordinary failure alone is not enough.
- Any level → `ui`/`assets` when UI, responsive behavior, visual QA, screenshots, images, illustrations, or icons become necessary.

On escalation, stop dispatching under the old workflow, record the evidence, re-run prerequisites, preserve verified work, and resume at the first missing stage of the stronger workflow. Do not repeat already sufficient work or duplicate implementation.
