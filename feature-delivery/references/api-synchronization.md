# Repository-driven API artifact synchronization gate

Use this reference only when API endpoints, contracts, authentication, requests, responses, or errors change **and** the repository designates an authoritative API artifact or workflow. This gate is tool-agnostic: it may cover Postman, OpenAPI, Bruno, or another artifact only when that artifact is authoritative in the repository.

Follow repository policy for the target artifact, commands, skills, approvals, and access. Update the designated artifact only when authorized. Do not create a duplicate collection, specification, workspace, or replacement workflow; do not infer a preferred tool or guess a target. Keep examples non-secret and synchronize the changed contract, authentication behavior, requests, responses, errors, and dependency-ordered flows that repository policy requires.

Verify synchronization using the repository-designated command or integration. If the repository has no authoritative API artifact, or the change does not trigger synchronization, record `API artifact gate: not applicable`. If a required authoritative artifact exists but access is unavailable, report the gate as blocked rather than passed.
