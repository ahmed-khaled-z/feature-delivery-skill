---
name: feature-delivery-setup
description: Use when configuring or changing the delegate-fleet.v1 lanes consumed by feature-delivery, including delegate tools, models, effort, variants, read-only behavior, or project overrides.
---

# Feature Delivery Setup

Use `delegate-fleet.v1` as the single configuration source. Do not create a separate feature-delivery mapping or write lane bindings into `AGENTS.md`.

Load `$delegate-setup` and follow it completely. Inspect the effective global/project fleet first. Preserve current bindings unless the user asks to change them, and never inject lane names, implementers, models, roles, or dials from this compatibility skill.

When the user requests a change, pass their desired responsibilities and constraints to `$delegate-setup`. It must discover actual CLI/model availability, present the full proposed table and exact JSON, identify scope and trust implications, and receive explicit approval before writing.

After an approved write, load the effective map again and report the active lanes, sources, project trust, unavailable bindings, and any requested capability that remains unsatisfied. Do not dispatch delivery work from this setup skill.
