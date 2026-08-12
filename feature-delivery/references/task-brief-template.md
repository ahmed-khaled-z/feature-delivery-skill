# Delegated task brief

Read this reference only immediately before writing or dispatching a delegated task. Write one compact English XML brief for the current task:

```xml
<task id="...">
  <goal>One bounded outcome.</goal>
  <current_state>Relevant repository evidence only.</current_state>
  <prerequisites>Passed prerequisites and landed dependencies.</prerequisites>
  <scope>Exact files or narrow surfaces allowed to change.</scope>
  <leave_untouched>Forbidden surfaces and confirmed decisions.</leave_untouched>
  <requirements>Deterministic behavior, edge cases, and constraints.</requirements>
  <acceptance_criteria>Observable pass conditions.</acceptance_criteria>
  <verification_loop>Real repository commands; fix failures.</verification_loop>
  <action_safety>No unrelated refactor; no git add, commit, push, deploy, or remote migration.</action_safety>
  <report_contract>At most eight bullets: changed files, behavior, tests, deviations, blockers.</report_contract>
</task>
```

The brief must include the selected lane, resolved delegate and fleet dials, routing reason, applicable gates, and actual repository verification commands. Omit unresolved optional dials rather than inventing a model or alias. Direct the implementer to stop on contradictions or newly missing prerequisites instead of guessing, expanding scope, changing confirmed architecture, or adding workarounds. Give exact target paths and leave-untouched surfaces, preserve unrelated changes, and keep a fresh brief below 600 words unless safety requires more; split it otherwise.

One brief covers one independently verifiable behavior or artifact and one review boundary. Stronger models do not justify broader briefs. The implementer edits; the orchestrator independently reviews, requests correction from the same implementer when needed, verifies, and lands.
