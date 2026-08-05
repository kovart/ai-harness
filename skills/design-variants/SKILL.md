---
name: design-variants
description: >
  Generate standalone HTML demo pages with multiple distinct design variants for UI components.
  Use to explore design directions in isolation before implementing. Auto-discovers the project's
  design tokens (CSS variables, fonts, colors, spacing, radii, shadows) and generates realistic
  mockups using them. Triggers: (1) user shares a screenshot and wants to redesign a component,
  (2) user wants to design a new component/section from scratch, (3) user says "design options",
  "show me variants", "redesign X", "I don't like how X looks", or similar design exploration requests.
  NOT for Figma-related work — those have dedicated skills.
---

# Design Variants

Generate standalone HTML demo pages with multiple design options for a UI component, using the project's real design tokens. The user picks and mixes elements across options until they settle on a direction, then you implement it.

## Workflow

### Phase 1 — Discover Design Tokens

Auto-scan the project for design tokens.

Extract and note:
- **Font family** (and Google Fonts URL if web font)
- **Color palette** — neutrals, brand, semantic (success/warning/danger)
- **Spacing scale** — base unit and named stops
- **Border radius** — scale from sm to full
- **Shadows** — elevation levels
- **Transitions/motion** — duration and easing tokens
- **Typography** — size scale, weight scale

If tokens are unclear or missing, ask the user for a pointer to the design system files.

### Phase 2 — Understand the Component

If a **screenshot** is provided, analyze it to understand the component's structure, data shape, layout, and current visual treatment.

If designing **from scratch**, ask 1-2 focused questions:
- What data does this component display?
- What tone/density/style fits? (e.g. "dense data table" vs "spacious dashboard card")

Read the existing component code if it exists — understand the data model, states (empty, loading, error, filtered), and interaction patterns.

### Phase 3 — Generate Demo Page

Create `{component-name}-design-options.html` at the project root.

**Page structure:**
- CSS variables block mirroring the project's exact tokens
- Load the project's font via `@import url(...)` or `@font-face`
- Page header with title and short description
- Each variant in its own labeled section

**Per variant:**
- Label with number badge, short name, and one-line description
- Full component context — not just the piece being redesigned, but headers, metrics, surrounding UI that gives it meaning
- Realistic mock data matching the actual data shape from the codebase
- CSS `:hover` / `:focus` states where relevant
- Multiple states shown as separate cards if relevant (e.g. "Default state" and "With filters applied")

**Variant count:** 7 by default. User can request more or fewer.

**Variant quality rules:**
- Each variant must be **structurally different** — different layout composition, information hierarchy, or visual treatment. Not just color/size tweaks.
- Consult `/frontend-design` and `/interface-design` skill guidelines for quality standards.
- No gradient text, no glassmorphism, no neon-on-dark unless the project's design system uses them.

Open the file in the browser after generating: `open {file}` (macOS) or `xdg-open {file}` (Linux).

### Phase 4 — Iterate

When the user gives feedback ("I like the badges from Option 3 and the bar from Option 5"):

1. Generate a new file: `{component-name}-design-v{N}.html`
2. Combine the selected elements into 2-3 refined variants
3. Show all relevant states for each refined variant
4. Open in browser

Repeat until the user picks a final direction.

### Phase 5 — Implement

When the user says "implement this" or picks a final option:
1. Implement the chosen design in the actual codebase
2. Reuse existing project components and primitives where possible
3. Extend primitives if needed to keep the project maintainable
4. Optionally clean up demo HTML files (ask the user)

## Demo Page Quality Checklist

See [references/demo-page-guidelines.md](references/demo-page-guidelines.md) for detailed HTML structure patterns, variant differentiation strategies, and common pitfalls.

## Key Principles

- Demo pages are **disposable artifacts** for the design conversation
- Always use the project's **real design tokens**, never generic values
- Variants must be **structurally different**, not superficial tweaks
- Show **full component context** — a table floating alone is a component demo, not a design
- Use **realistic data** matching the actual data shape from the codebase
- Static HTML with CSS hover states only — no JavaScript behavior needed
