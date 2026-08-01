---
name: feature-delivery-setup
description: Configure the delegate policy used by feature-delivery. Discover installed Codex, OpenCode, and Kimi delegate tools; let the user choose which are authorized; discover configured OpenCode models for an explicit allowlist; and save the result for the current conversation, project AGENTS.md, or global user defaults. Use when the user invokes $feature-delivery-setup, asks to configure feature-delivery delegates, or needs to change allowed OpenCode models.
---

# Feature Delivery Setup

Configure authorization explicitly. Tool presence proves availability, never consent. Match the user's language and use structured choice UI when the host supports it; otherwise ask concise numbered questions.

## 1. Inspect without mutation

Determine the current working repository and read the nearest `AGENTS.md` if present. Read the global config at `~/.config/feature-delivery/config.json` if present. Preserve unrelated files and policy.

Discover only these supported delegate skills without loading their full instructions:

- `codex-delegate`
- `opencode-delegate`
- `kimi-delegate`

Use the host's installed-skill catalog or inspect its standard skill directories read-only. Optionally check each matching executable with a read-only version command. Do not install, authenticate, edit configuration, or expose secrets. Show available, unavailable, and already configured delegates separately.

## 2. Choose delegates

Ask which available delegates the user authorizes. Allow one or more choices. Do not enable a delegate merely because it is installed, mentioned by conditional repository guidance, or was enabled in an older scope.

Apply these policies:

- Codex: save only enabled/disabled. `feature-delivery` owns automatic per-task model selection.
- Kimi: save only enabled/disabled. Preserve its configured/default alias.
- OpenCode: if enabled, complete the model allowlist step before saving.

## 3. Build the OpenCode allowlist

When OpenCode is authorized, verify that `opencode-delegate` and the `opencode` executable are available, then run:

```bash
opencode models
```

Treat this as read-only discovery. Present the exact returned model identifiers without renaming, guessing prices, or preselecting them. Ask the user to choose one or more identifiers; the selected set is the complete allowlist. Never treat discovered models as authorized automatically. If discovery fails, report the error and do not save OpenCode as enabled.

## 4. Choose scope and preview

Ask where to apply the policy:

1. Current conversation: make no filesystem change; state the resolved policy for subsequent `feature-delivery` use.
2. Current project: add or replace only the marked block below in the nearest `AGENTS.md`. Create `AGENTS.md` only after the user confirms the preview.
3. Global default: write `~/.config/feature-delivery/config.json`. Create only its parent directory if needed and do not touch other user configuration.

Before any write, show the exact target and complete proposed policy, then obtain confirmation. Invocation of this skill alone does not authorize persistence.

Use this project block and preserve all content outside the markers:

```md
<!-- feature-delivery:delegation:start -->
## Feature Delivery Delegation

- Codex: enabled; model selection is automatic per task.
- OpenCode: enabled only with these user-approved models:
  - provider/model
- Kimi: disabled.
<!-- feature-delivery:delegation:end -->
```

Omit the OpenCode model sub-list when OpenCode is disabled. Use this exact global JSON shape with booleans for every supported delegate:

```json
{
  "version": 1,
  "delegates": {
    "codex": { "enabled": true },
    "opencode": {
      "enabled": true,
      "allowedModels": ["provider/model"]
    },
    "kimi": { "enabled": false }
  }
}
```

When OpenCode is disabled, save an empty `allowedModels` array. Validate JSON after writing.

## 5. Report

Report enabled delegates, the OpenCode allowlist when applicable, scope, and written path. Remind the user to start a new agent conversation if an already-open conversation cached older skill instructions. Do not dispatch implementation from this setup skill.
