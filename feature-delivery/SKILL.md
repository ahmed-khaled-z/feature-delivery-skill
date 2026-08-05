---
name: feature-delivery
description: Analyze rough feature requests, resolve ambiguity, normalize requirements into English implementation specifications, decompose work into dependency-ordered tasks, cost-balance model allocation, route bounded work through approved delegates, and deliver through independent review, correction, verification, and safe landing. Use when the user asks to build, implement, start, continue, or plan a software feature with a prerequisite-first workflow.
---

# Feature Delivery

Deliver one feature without skipping foundations, inventing architecture, overfilling an implementer context, or trusting delegated work blindly. Repository and user rules are authoritative over this skill.

For an implementation or delegation request, resolve delegate authorization before feature analysis. Use explicit policy from the current request, then the nearest `AGENTS.md`, then `~/.config/feature-delivery/config.json`. A conditional rule such as “when delegating UI to OpenCode” does not enable that delegate. If no source explicitly enables at least one delegate, stop and ask the user to run `$feature-delivery-setup`; do not silently implement directly. Planning-only requests may proceed with an unassigned plan.

## 1. Analyze and ground the request

Accept rough requirements in any language. Preserve the original request and its language for discussion. Extract outcome; actors; inputs, outputs, rules, states, and edge cases; scope and exclusions; acceptance signals; labeled inferences; and unknowns that could change behavior, architecture, data, UX, cost, security, or scope.

Before planning, read the nearest `AGENTS.md`; branch, status, recent commits, and relevant diff; architecture, ERD, status, product, design, and execution docs named by repository rules; relevant code, tests, scripts, migrations, config, and deployment setup. Preserve unrelated changes. Never rewrite confirmed architecture or schema decisions without explicit approval.

Prefer repository evidence over inference. Do not translate the request literally or delegate it before the next two gates pass.

## 2. Run the prerequisite and ambiguity gates

Identify earlier delivery steps; schema/migrations; APIs/contracts/clients; config, credentials, providers, and infrastructure; UI foundations; test, fixture, observability, security, and deployment needs. Compare each with repository evidence.

If any prerequisite is missing, stop before planning or delegation. State what is missing, why it is required, the correct execution order, and any needed decision, approval, credential, or external confirmation. Do not silently add scope or invent a workaround. Resume only after the prerequisite is complete or scope changes explicitly.

Classify remaining unknowns as **blocking** when answers could change behavior, architecture, data, UX, cost, security, or scope, otherwise **non-blocking** only if a safe, reversible default fits repository truth. State non-blocking assumptions. Ask the smallest high-leverage question in the user's language for a blocking ambiguity and pause.

Once both gates pass, create the canonical English specification:

```text
Outcome:
Users and behavior:
In scope:
Out of scope:
Repository evidence and constraints:
Acceptance criteria:
Assumptions and resolved decisions:
Verification expectations:
```

Keep user-facing explanations in the user's language unless requested otherwise.

## 3. Decompose and score

Build a dependency-ordered task graph. Keep one active task by default. Each task has an ID, one concrete outcome, dependencies, exact scope, acceptance criteria, verification artifact, 1–5 complexity/importance/risk scores, delegate, exact model or alias, and selection reason.

- One task means one independently verifiable behavior or artifact and one review boundary. Split broad screens into coherent components and order foundations/contracts before consumers.
- Keep the repository buildable after every landed task. Parallelize only tasks with no shared decisions or files.
- Give exact target paths and leave-untouched surfaces. A fresh brief stays below 600 words unless safety requires more; split it otherwise.
- Score complexity from mechanical (1) to architectural/concurrent/novel (5); importance from cosmetic/internal (1) to auth, money, security, privacy, or irreversible impact (5); and risk from trivial rollback (1) to severe loss, exploit, or production-integrity exposure (5).
- Split for coherence before escalating capability. Do not inflate scores to choose a preferred model.
- Apply the mandatory cost-balance portfolio rule in [delegation-routing.md](references/delegation-routing.md): at least `ceil(N / 2)` of the `N` dispatched implementation tasks must use models outside the Kimi K3, ChatGPT Sol, and GLM 5.2 model families. Do not split work artificially to satisfy the ratio.

Present the concise queue before implementation. Every row must include: `task | C/I/R | delegate | exact model/alias | model class | reason`. Include the planned non-premium ratio and validate it before the first dispatch.

## 4. Route delegates and models

Use the policy resolved before analysis. Precedence is current request, nearest `AGENTS.md`, then the global config. Installed skills or CLIs are availability evidence, not approval. A conversation-only selection expires with that conversation.

If OpenCode is enabled without an explicit allowed-model set, ask for that allowlist before routing any task to OpenCode. Codex needs no model question because the orchestrator selects its model from verified available models based on the task. Kimi keeps its configured alias. Never infer approval from `opencode models`, silently substitute an unavailable, unauthenticated, disallowed, billed, or weaker tool/model, or authenticate during discovery.

Load only the selected delegate skill immediately before preflight and dispatch. When selecting a delegate or model, read [delegation-routing.md](references/delegation-routing.md) for the full ownership, precedence, model-selection, alias, and selected-tool preflight rules.

## 5. Apply repository-driven conditional gates

Determine applicable UI, API-artifact, security, migration, and documentation gates from repository policy and the canonical specification. Mark an inapplicable gate explicitly; do not add a project-specific workflow by default.

- For user-facing UI/UX work, read [ui-delivery.md](references/ui-delivery.md) only when the repository makes that gate relevant.
- For API changes, read [api-synchronization.md](references/api-synchronization.md) only when the repository identifies an authoritative API artifact such as Postman, OpenAPI, or Bruno.
- Apply other repository-required gates only when triggered. Never apply remote migrations, deploy, push, publish, or mutate an external system without explicit authorization; unavailable required access is blocked, not passed.

## 6. Brief, preflight, and delegate

Preflight only the selected tool and its required authentication/configuration; do not probe unrelated CLIs. Immediately before writing or dispatching a delegated task, read [task-brief-template.md](references/task-brief-template.md) and produce one compact English XML brief for the current task. The implementer edits; the orchestrator reviews and lands.

## 7. Review, correct, verify, and land

Never trust an implementer's report. Inspect changed existing tests first. Read the full diff against the brief for scope creep, missing behavior, architecture drift, weakened coverage, unapproved contract changes, swallowed errors, speculative abstractions, duplication, and unverified APIs. Re-run relevant repository gates yourself. Round-trip schema changes against a scratch DB; never apply remote migrations without authorization.

For a relevant UI gate, verify real flows, responsive layouts, RTL/LTR as applicable, keyboard access, and required states. Run installed guard skills when relevant. If a defect exists, send a concise English delta brief to the same implementer session first; do not fix it directly unless the user changes the workflow. Review again after correction. On a repeated failure, rescore, split where possible, and escalate only the remaining bounded work; record the failure reason.

Land each verified task before dependent work. Carry confirmed constraints forward. Recompute the cost-balance ratio whenever a task is added, split, cancelled, blocked, or reassigned, and validate the final ratio over completed implementation tasks. After the queue, run a feature coherence check and all applicable gates. Sync only repository-authoritative docs and artifacts. Commit only when authorized; push, deploy, alter external tracking, or migrate remote data only with explicit authorization. Report delivered outcome, queue/model summary, final non-premium ratio, decisions, verification, applicable-gate results, commit/deployment state, and blockers separately.
