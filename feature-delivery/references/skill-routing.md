# Dynamic skill routing

Use the smallest exact set of skills whose descriptions match the delegated task. Inventory the target delegate's current Skills Library before writing the brief; names on the orchestrator are only candidates until availability is confirmed. A shared local skill may be used by a delegate that does not register it natively only when the orchestrator resolves its canonical `SKILL.md` path and preflights that the delegate can read it.

## Required baseline

Every dispatch—including planning, implementation, testing, diagnosis, review, review-fix, documentation, and design—includes:

- `ponytail ultra` for a mechanical `quick` T0 task;
- `ponytail full` for T1–T4 and any non-mechanical task.

Ponytail minimizes implementation, not understanding or required safeguards.

## Superpowers selection

Choose by observable trigger, not by tier alone:

| Trigger | Exact skill |
| --- | --- |
| New feature, creative behavior, ambiguous requirements, or solution shaping | `brainstorming` |
| Approved design/spec with multiple implementation steps | `writing-plans` |
| Bug, failing test/build, unexpected behavior, or unknown root cause | `systematic-debugging` |
| Feature, bug fix, refactor, or behavior change with a usable test harness | `test-driven-development` |
| Evaluating and applying review findings | `receiving-code-review` |
| Before claiming a nontrivial delegated task is complete | `verification-before-completion` |

Follow each selected skill's reasoning and approval gates. The user's instructions, repository policy, feature-delivery's mandatory delegation invariant, and external-mutation authorization remain authoritative when a process skill assumes a different execution harness.

Stage ownership matters:

- The orchestrator invokes `brainstorming` and mediates its user-approval gate before any mutation. A planning dispatch may provide repository evidence or options but cannot approve itself.
- If brainstorming classifies the work as bounded, keep the approved design in chat and proceed to bounded task briefs; do not create a plan document merely because delivery mode is `full`.
- If brainstorming classifies it as architectural, write the approved spec through a compatible writable lane, then use `writing-plans` through a compatible planning lane. Do not let the orchestrator author the repository artifact directly.
- A Superpowers instruction to commit is conditional on current user authorization. Without it, record the proposed commit boundary but do not commit.
- `writing-plans` supplies plan structure and task decomposition. Its execution handoff is replaced by feature-delivery's lane dispatch unless the user explicitly authorizes nested delegation for this run.
- Implementation receives the approved design/plan plus `test-driven-development` when applicable. `systematic-debugging` requires root-cause evidence before a fix.

Feature delivery already owns orchestration. Do not select `dispatching-parallel-agents`, `subagent-driven-development`, or `executing-plans` inside a delegate by default. Do not use `requesting-code-review`; this workflow uses `debate-review`. Select branch/worktree/finishing skills only when the user separately requests that lifecycle or the repository workflow requires it.

## UI/UX and visual work

Any UI/UX design, layout, styling, responsive behavior, accessibility presentation, interaction, or visual implementation includes both. Copy-only and nonvisual behavior edits do not trigger this pair:

- `ui-ux-pro-max` for evidence-driven design-system, UX, accessibility, and platform guidance;
- `frontend-design` for a coherent, distinctive visual direction and implementation quality.

The repository's platform, component library, design system, and explicit user constraints remain authoritative. Apply web-specific advice from `frontend-design` only where it fits the actual stack.

## Missing skills

If a required skill is not registered natively, resolve its canonical local `SKILL.md` and pass the absolute source path in the task brief. If the delegate cannot read that resource, try another eligible current lane. If none can load it, stop and report the missing skill and target environment. Do not paste an improvised summary and claim the skill was used.
