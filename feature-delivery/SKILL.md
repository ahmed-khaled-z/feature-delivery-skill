---
name: feature-delivery
description: Use when the user asks to build, change, fix, refactor, or deliver repository software through a configured delegate fleet, including quick low-risk edits and medium or large features.
---

# Feature Delivery

The active host is the orchestrator. It grounds the request, chooses the mode and T0–T4 tier, resolves the current fleet, dispatches bounded work, inspects every result, and performs final acceptance. It does not author repository mutations unless the current user explicitly says to implement directly or not delegate.

## Commands

- `$feature-delivery quick <request>` — a fast path for an explicitly small, reversible, low-risk T0/T1 change. Use one implementation dispatch and the narrowest credible verification. Escalate automatically to `full` before the next mutation if repository evidence reveals broader risk.
- `$feature-delivery full <request>` — the default when the mode is omitted. Use for new features, T2–T4 work, cross-file behavior, architecture, migrations, security, data, or anything whose risk is not clearly negligible.

`quick` changes ceremony, never safety. Authentication, authorization, payments, migrations, destructive operations, sensitive data, concurrency, shared contracts, or meaningful architectural decisions cannot remain in `quick`.

## Non-negotiable invariants

1. Load the effective `delegate-fleet.v1` map at run time. Never encode lane names, implementers, models, or effort values in this skill.
2. Every repository mutation—code, tests, configuration, migrations, docs, and assets—must be produced through a compatible current fleet lane before the artifact changes.
3. Every delegated task requires `ponytail`; use `ultra` only for a genuinely mechanical `quick` T0 task and `full` otherwise. Ponytail never removes security, validation, error handling, accessibility, or explicit requirements.
4. Choose model reasoning effort from the task tier when the selected relay/model supports an override. Preserve the configured lane dial when support cannot be verified; never invent a value.
5. Select supporting skills from the target delegate's current Skills Library by exact name and task semantics. Do not assume a skill is installed merely because it exists on the orchestrator.
6. UI/UX design, layout, styling, responsive, accessibility-presentation, interaction, or other visual implementation requires both `ui-ux-pro-max` and `frontend-design` in the delegate brief. A copy-only or nonvisual behavior edit does not trigger this gate.
7. T2–T4 work and any risk-triggered T0/T1 work use `debate-review`. Fixing accepted review findings uses the review lifecycle in [review-lifecycle.md](references/review-lifecycle.md).
8. Treat delegate reports as claims. Inspect the diff and run fresh verification before accepting or landing work.

## Workflow

1. **Ground.** Read the nearest `AGENTS.md`, repository status and relevant diff, named design/product/architecture docs, and the code/tests/configuration that control the requested behavior. Preserve unrelated changes.
2. **Classify.** Read [task-classification.md](references/task-classification.md). Record mode, tier, and one evidence-based reason. Ask only for a decision that materially changes scope, safety, or external effects.
3. **Shape.** For reasoning-heavy work, read [skill-routing.md](references/skill-routing.md) and apply the exact Superpowers reasoning and approval gates before implementation. Use that reference's ownership adaptation when a process skill assumes direct writes, commits, or nested execution.
4. **Resolve.** Read [delegation-routing.md](references/delegation-routing.md), load the live fleet, rank eligible lanes by capability and semantic fit, then preflight only the selected relay.
5. **Preview.** Show the user the mode, tier, reason, and material tasks in execution order:

   ```text
   Task | Purpose | Lane | Implementer | Model | Effort/variant | Ponytail | Skills | Status/condition
   ```

   Use exact values from the live fleet and the selected one-off dial. Write `configured CLI default (not pinned)` when no model is pinned. Continue automatically unless an approval gate, missing prerequisite, safety decision, or external mutation requires an answer.
6. **Dispatch.** Immediately before each dispatch, read [task-brief-template.md](references/task-brief-template.md). Keep one owner per surface and retain lane, relay result/session id, and touched-file evidence.
7. **Inspect and verify.** Review the full diff for scope, architecture, tests, regressions, security, performance, concurrency, accessibility, and unsupported APIs. Run the repository's real checks yourself.
8. **Review and correct.** Apply [review-lifecycle.md](references/review-lifecycle.md) when review is required. Reclassify and re-preview remaining work if risk or routing changes.
9. **Accept.** Check every acceptance criterion with fresh evidence. Commit, push, deploy, comment, resolve threads, or mutate external systems only when authorized.

Read [orchestration-workflows.md](references/orchestration-workflows.md) for the two mode workflows and escalation rules. Load [ui-delivery.md](references/ui-delivery.md) only for user-facing UI/UX or visual work, and [api-synchronization.md](references/api-synchronization.md) only when the repository names an authoritative API artifact.

## Dispatch failure

If the selected lane, relay, authentication, model, or required skill is absent or incompatible, try another eligible lane only when it preserves capability, independence, safety, and the user's stated constraints. Announce any material reroute. If no compatible lane exists, stop and report the exact blocker; never silently edit directly.

## Completion report

Report the final mode and tier, any escalation, actual lanes/models/dials used, selected skills, delivered behavior, verification evidence, review/fix status, documentation impact, landing state, and unresolved blockers.
