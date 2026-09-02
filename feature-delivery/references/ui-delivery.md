# UI/UX delivery gate

Use this gate for UI/UX design, layout, styling, responsive behavior, accessibility presentation, visual assets, or interaction-state work. Do not trigger it for a copy-only change or nonvisual behavior that happens to live in a UI file.

1. Ground the repository's design system, component library, platform conventions, assets, and visual acceptance evidence.
2. Require both `ui-ux-pro-max` and `frontend-design` in the delegated task brief, plus the tier-appropriate Ponytail instruction.
3. Use `ui-ux-pro-max` to derive the smallest relevant UX/design-system guidance. Use `frontend-design` to establish a deliberate visual direction and production-quality implementation. Repository and platform rules override generic web advice.
4. Resolve a compatible writable lane dynamically. A visually named lane is a ranking signal, not a fixed dependency. Do not give the same surface to a second implementation lane.
5. Put measurable layout, spacing, typography, color, responsive, RTL/LTR, accessibility, interaction, loading, empty, error, disabled, and permission states in the acceptance criteria when applicable.
6. Verify the real flow at repository-required viewports and states, including keyboard/focus behavior, contrast, reduced motion, and RTL/LTR where supported.

Reuse suitable existing assets before generating new ones. Any asset generation/editing still follows the repository's required tools, licenses, and approval rules.
