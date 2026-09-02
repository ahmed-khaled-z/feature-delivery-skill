# Quick and full workflows

Every repository mutation in either mode is delegated through a dynamically resolved current lane. The orchestrator may inspect, decide, and verify; it does not patch implementation artifacts directly.

## `quick` — T0/T1 only

Use when the user explicitly chooses `quick` and grounding confirms the change is isolated, reversible, low-risk, and directly verifiable.

1. Ground the exact surface and confirm T0 or T1.
2. Produce a two-to-four sentence bounded intent. If a selected process skill imposes approval, obtain it.
3. Resolve one compatible writable lane dynamically.
4. Dispatch one narrow brief with `ponytail ultra` for mechanical T0, otherwise `ponytail full`.
5. Inspect the diff and run the narrowest existing relevant check.
6. Update docs through a separate dynamically selected lane only if observable documented behavior changed.
7. Run review only when a review trigger below appears.

Do not create broad test infrastructure for a literal mechanical T0 edit. Behavior-changing T1 work still follows repository testing policy and uses test-first delivery when a test harness exists.

## `full` — default, T2–T4 and substantial features

1. Ground dependencies, architecture, data flow, tests, UI rules, and acceptance criteria.
2. Select the exact Superpowers process skills from [skill-routing.md](skill-routing.md). New or ambiguous features start with `brainstorming`. A bounded approved design proceeds directly to task briefs; an architectural approved design proceeds to a delegated spec and `writing-plans`.
3. At T3/T4, dynamically select distinct plan/challenge lanes when available and run a bounded architecture debate. The orchestrator records accepted, partially accepted, and rejected objections with evidence.
4. Split the approved plan into independently verifiable, non-overlapping tasks. Resolve one current lane per task.
5. Dispatch implementation. Keep one owner per surface; parallelize only tasks with no shared files or unresolved decisions.
6. For UI/UX/visual work, apply [ui-delivery.md](ui-delivery.md).
7. Inspect all diffs and run build, analysis, tests, and task-specific checks.
8. Run `debate-review` on the combined change.
9. Validate each finding, then apply [review-lifecycle.md](review-lifecycle.md) for accepted fixes and re-review.
10. Delegate affected documentation only after behavior is stable, then run a final landing check.
11. Verify every acceptance criterion and report evidence.

## Review triggers

Review is mandatory for T2–T4 and for any lower-tier change involving unexpected scope, shared behavior, a public contract, security/privacy, state/concurrency, data/schema, performance risk, unfamiliar integration, or nontrivial UI interaction. A purely mechanical T0 and a contained well-tested T1 may omit review, with the omission stated in the final report.

## Dynamic escalation

- `quick` → `full` when the task exceeds T1 or gains a review trigger.
- T0/T1 → T2 when multiple coupled surfaces, shared behavior, or independent review becomes necessary.
- T2 → T3 for architecture, cross-module state/data flow, concurrency, major refactoring, or substantial regression risk.
- Any tier → T4 for authentication/authorization, payments, sensitive data, migrations, destructive/irreversible operations, or serious production impact.
- Any implementation → systematic diagnosis when the root cause is unknown or a meaningful failure persists after one evidence-based correction and rerun.

On escalation, stop new dispatches under the old workflow, preserve verified work, re-run prerequisites, select any newly required skills, show a revised preview, and resume at the first missing stage. Do not duplicate completed work.
