# Review and fix lifecycle

## Independent review

Use `debate-review` for every review required by [orchestration-workflows.md](orchestration-workflows.md). Dynamically select two distinct eligible read-only review lanes from the live fleet and pass them through `debate-review`'s `--main-lane` and `--debate-lane` options:

- use its local-working-tree mode when there is no PR/MR;
- use the repository's PR/MR target when one exists and remote review actions are authorized;
- run its dry-run/preflight first when configuration or posting behavior is uncertain.

The tool's built-in lane names are defaults only; do not rely on them. Preflight both resolved lanes before the review gate and keep them independent from the implementation owner/session and from each other as required by `debate-review`. Always pass both lane flags. The runner has no per-review effort/variant override, so reviewers use their live configured lane dials; disclose this in the preview. Check relay results for touched files and read-only violations. If two compatible reviewers are unavailable, report the missing independent review capability rather than silently substituting the implementer.

The orchestrator validates each finding against repository evidence before accepting it. Reject technically incorrect, duplicative, speculative, or out-of-scope findings with a concise reason.

## Fixing reviewed work

Classify every validated finding by actual consequence before scheduling it. Every accepted review fix brief includes `receiving-code-review`, `ponytail full`, and any task-specific skill. When the root cause is unknown, dispatch read-only `systematic-debugging` first and do not issue a writable fix brief until the evidence identifies the cause. Code mutations remain delegated through a dynamically selected writable fix-capable lane.

When a GitHub or GitLab PR/MR exists, use `babysit-pr` to coordinate the review lifecycle: harvest active threads, validate findings, track rounds, reply in the correct thread, and resolve only verified fixes. Feature delivery's explicit delegation and authorization boundaries override `babysit-pr` defaults that assume the active agent fixes or pushes directly. Its presence does not authorize direct orchestrator edits, commits, pushes, replies, resolutions, or other remote mutations. Delegate each code fix through the selected current lane, verify it, then let `babysit-pr` continue the authorized parts of the round.

When there is no PR/MR, `babysit-pr` is not applicable. Delegate accepted fixes through the dynamically selected lane, verify them, and rerun `debate-review --local` until there are no validated blocking findings or a blocker requires user input.

For review systems that `babysit-pr` cannot harvest, relay the findings explicitly into the same validate → delegate fix → verify → re-review loop.

Do not ask the reviewer to implement its own findings. Preserve independent reviewer and implementer sessions.
