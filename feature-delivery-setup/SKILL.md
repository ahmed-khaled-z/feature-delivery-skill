---
name: feature-delivery-setup
description: Compatibility entrypoint for configuring the delegate-fleet.v1 lanes consumed by feature-delivery. Use when the user invokes $feature-delivery-setup, asks to configure or change feature-delivery delegates/models, or needs the expected explore, fast, feature, tests, debug, ui, assets, review, architecture, and docs lanes.
---

# Feature Delivery Setup

Use `delegate-fleet.v1` as the single configuration source. Do not create or update the obsolete `~/.config/feature-delivery/config.json`, and do not write delegation policy into `AGENTS.md`.

Load the installed `$delegate-setup` skill and follow it completely. Tell it the desired responsibility map is:

| Lane | Responsibility | Preferred binding |
| --- | --- | --- |
| `explore` | Read-only repository evidence | Kimi standalone |
| `fast` | T0/T1 implementation | Kimi standalone |
| `feature` | Main implementation and fixes | Kimi standalone |
| `tests` | Tests and bounded mechanical work | OpenCode |
| `debug` | Difficult debugging only | OpenCode + `zai-coding-plan/glm-5.2` |
| `ui` | UI/responsive/visual QA | Antigravity |
| `assets` | Images/icons/illustrations/assets | Antigravity |
| `review` | Independent implementation review | Codex |
| `architecture` | T3/T4 adversarial critique | Codex `gpt-5.6-sol`, effort `high` |
| `docs` | Documentation after stability | OpenCode + `opencode/deepseek-v4-flash-free` |

Discovery proves availability, not authorization or task fit. Preserve existing effective bindings unless the user approves a change. `$delegate-setup` must show its table and exact JSON, ask global versus project scope, and obtain explicit approval before writing.

After setup, load the effective map and report active lanes, their sources, project trust, unavailable bindings, and any deviation from the preferred responsibility map. Do not dispatch work from this compatibility skill.
