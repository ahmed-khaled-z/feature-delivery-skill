# Delegated task brief

Read this reference immediately before every dispatch. Write one bounded English brief below 700 words unless safety requires more; split larger work into separate review boundaries.

```xml
<task id="...">
  <delivery mode="quick|full" tier="T0|T1|T2|T3|T4" />
  <route lane="exact-live-lane" implementer="exact-live-implementer" model="exact-live-model-or-default" reasoning="exact-effort-or-variant-or-configured-default" />
  <required_skills>
    <skill name="ponytail" source="native-or-resolved-SKILL.md" argument="ultra|full" reason="minimum complete solution" />
    <skill name="exact-skill" source="native-or-resolved-SKILL.md" reason="observable task trigger" />
  </required_skills>
  <goal>One independently verifiable outcome.</goal>
  <current_state>Relevant repository evidence only.</current_state>
  <prerequisites>Passed prerequisites and landed dependencies.</prerequisites>
  <scope>Exact files or narrow surfaces allowed to change.</scope>
  <leave_untouched>Unrelated changes, forbidden surfaces, and confirmed decisions.</leave_untouched>
  <requirements>Deterministic behavior, edge cases, constraints, and approved design.</requirements>
  <acceptance_criteria>Observable pass conditions.</acceptance_criteria>
  <verification_loop>Real repository commands and expected evidence; fix failures within scope.</verification_loop>
  <action_safety>No unrelated refactor; no git add, commit, push, deploy, remote mutation, or nested delegation.</action_safety>
  <report_contract>Changed files, behavior, skill use, tests/evidence, deviations, and blockers in at most eight bullets.</report_contract>
</task>
```

The delegate must load every named skill before acting and explicitly state which ones it applied. Every brief includes Ponytail. Prefer a skill registered natively in the target environment. Otherwise resolve the canonical local `SKILL.md`, confirm the delegate can read it, and include its absolute path as `source`; the delegate reads that file and its required references completely before acting. Include both `ui-ux-pro-max` and `frontend-design` for visual or interaction design implementation; copy-only and nonvisual behavior edits do not trigger them. Include the minimal exact set of Superpowers skills whose triggers apply to this dispatch stage. Do not send `brainstorming` to an implementation task after the design is approved, and do not send `writing-plans` as an implementation instruction.

Unknown-root-cause review work uses two briefs: a read-only diagnosis brief with `systematic-debugging`, followed only after evidence by a writable fix brief with `receiving-code-review`, applicable TDD, and completion verification. Do not collapse diagnosis and mutation into one speculative task.

The task brief owns requirements; the lane owns execution. A stronger model does not justify a broader brief. Direct the delegate to stop on contradictions or newly missing prerequisites instead of guessing, changing confirmed architecture, expanding scope, or adding workarounds.

Do not ask a delegate to spawn other agents or invoke Superpowers' nested-orchestration skills. Feature delivery remains the sole orchestrator unless the user explicitly approves nested delegation for this run.
