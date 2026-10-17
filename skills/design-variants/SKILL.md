---
name: design-variants
description: >
  Generate static HTML design rounds with multiple distinct design variants for a UI component, each
  drawn inside the full app page (shell, page header, neighbouring blocks) so it fits the product's
  design, compared side by side in an overview and openable as a full page. Use to explore design
  directions before implementing. Auto-discovers the project's design tokens (CSS variables, fonts,
  colors, spacing, radii, shadows) and generates realistic mockups using them. Triggers: (1) user
  shares a screenshot and wants to redesign a component, (2) user wants to design a new
  component/section from scratch, (3) user says "design options", "show me variants", "redesign X",
  "I don't like how X looks", or similar design exploration requests.
  NOT for Figma-related work — those have dedicated skills.
---

# Design Variants

Generate design rounds with multiple design options for a UI component, using the project's real design
tokens. Every option is a full app page with the component in place, so the user judges it in the real
context and picks and mixes elements across options until they settle on a direction; then you
implement it.

## Mode: Pixels, Not Programs

Phases 1–4 are design work. Spend the effort on composition, hierarchy, spacing, alignment, type,
color and finish, not on code. Engineering starts in Phase 5.

- **Hand-write each round as a folder of static `.html` files**: one full app page per variant and an
  `index.html` overview that frames them all. Shared tokens and page styles live in linked stylesheets
  (or the project export's two stylesheets); each variant's own styles stay inline; icons are inline
  SVG. No generator scripts (`.mjs`, `.js`, `.ts`, `.tsx`, `.py`), no React/Vite harness, no imports
  from `node_modules`, no build step, no JavaScript in the pages.
- **Existing components come from the project's export, never from memory.** When `package.json`
  has a `design:export` script, export just what the round needs, including the app shell (sidebar,
  top bar) and the page's neighbouring components:
  `npm run design:export -- --list --ids '<glob>'` shows ids and states, then
  `npm run design:export -- --ids app-shell,button,metric-card:default --out design/{component-name}-system`.
  Link its `tokens.css` and `styles.css`, copy each element verbatim from the fragments the run
  prints, and edit only what the variant redesigns. Never retype a component or its class names.
  Running this command is fine; writing scripts is not. Freeze hover, pressed or focus by adding
  `is-hover`, `is-active` or `is-focus-visible` to the element that has the pseudo-class. A
  component or state the export lacks is drawn by hand, labelled "hand-drawn" in its variant, and
  named in your reply so it can be added to the design system.
- **Start fresh.** Do not open or reuse rounds from earlier conversations in `design/` unless the user
  names one. Iterations build on this conversation's previous round.
- **Every variant lives in the full app.** Each variant page is the complete screen the component
  belongs to: app shell, top bar, page header and neighbouring blocks with plausible data, at a desktop
  viewport (1440×900 unless the project targets another). Copy the shell and neighbours from the
  export when there is one; otherwise hand-draw them from the layout and page code at the same
  fidelity as the variant, so nothing reads as a wireframe. No pixel diffs against the running app.
- **Charts are hand-drawn inline SVG.** Write the path data directly; approximate is fine. Don't load
  chart libraries. If the user names one (e.g. recharts), match its look: the library is an
  implementation choice.
- **Motion is CSS only**: transitions and `@keyframes`. Show behavior CSS can't express as labeled
  static frames.
- **Data is plausible, not verified.** Use the codebase's data shapes and realistic values. Don't call
  APIs, capture live responses, or audit data correctness.
- **Check once per round.** One screenshot pass to catch broken layout or overflow: a full-page
  screenshot of the overview shows every frame. No measure, film, or diff scripts.
- **No orchestration.** Don't bring in subagents, Codex, or review pipelines on your own. If the user
  asks to delegate, each helper hand-writes whole variant pages from the round's baseline under these
  rules.
- **No engineering gates.** No tests, lint, verification commands, tickets, or commits in Phases 1–4.
- **"Looks cheap" or "prototype-like" is a pixel problem.** Fix spacing, type scale, rhythm, edges and
  token use by hand. Don't switch to mounting real components.

## Fit the App

A variant that only looks good on its own fails. Design every variant as a part of the existing product:

- **The page around the component is fixed.** Shell, page header, neighbouring blocks and data are
  identical in every variant, so the only difference between options is the component itself. A
  variant that must move or change a neighbour says so in its hint.
- **Speak the app's design language.** Use its type scale, spacing rhythm, radii, surfaces, borders,
  elevation, density, icon set and existing components for every control that isn't being redesigned.
  Variants differ in structure (layout, hierarchy, component choice), not in visual style.
- **Sit on the page's grid.** Column widths, gutters, section spacing, heading levels and card padding
  match the neighbours; edges align with the blocks above and below.
- **Judge it in the frame.** Before finishing a variant, look at it inside the full page: if it pulls
  more attention than the page's primary content, looks heavier or lighter than its neighbours, or
  introduces a treatment found nowhere else in the app, rework it.
- **Departures are deliberate.** A new pattern or token is allowed only when the brief asks for it;
  mark it in the variant's hint and name it in your reply.

## Artifact Location

Keep every round in its own folder under the project-root `design/` folder, including iterations.
Create the folders as needed; resolve relative token, font, and asset URLs from the round folder.

In a Git checkout, verify each round with `git check-ignore -v -- design/{round}/index.html`. If it is
not ignored, add `/design/` once to the local exclude file returned by
`git rev-parse --git-path info/exclude` and recheck. `git ls-files -- design/{round}` must return
nothing: never overwrite tracked files or force-add these artifacts. Outside Git, use the same
`design/` folder.

## Workflow

### Phase 1 — Discover Design Tokens and the App Frame

Auto-scan the project for design tokens.

Extract and note:
- **Font family** (and Google Fonts URL if web font)
- **Color palette** — neutrals, brand, semantic (success/warning/danger)
- **Spacing scale** — base unit and named stops
- **Border radius** — scale from sm to full
- **Shadows** — elevation levels
- **Transitions/motion** — duration and easing tokens
- **Typography** — size scale, weight scale

Read the token source files and copy the values by hand, or link the project export's `tokens.css`.
Don't extract tokens from the running app or with scripts of your own. If tokens are unclear or missing, ask the user for a pointer to the design system files.

Then find the frame: the layout components (sidebar, top bar, page container widths) and the page the
component lives on, with the blocks around it. Note any design guide the project keeps (for example a
`DESIGN.md`); its rules bind every variant.

### Phase 2 — Understand the Component

If a **screenshot** is provided, analyze it to understand the component's structure, data shape, layout, and current visual treatment.

If designing **from scratch**, ask 1-2 focused questions:
- What data does this component display?
- What tone/density/style fits? (e.g. "dense data table" vs "spacious dashboard card")

Skim the existing component code, if any, for its data shape, states (empty, loading, error, filtered), and interaction patterns. This informs the mockup; it is not a source to import.

### Phase 3 — Build the Round

Create the round folder `design/{component-name}-options/`:

1. **Baseline page.** Build the full app page the component lives on: shell, top bar, page header,
   neighbouring blocks and plausible data. When redesigning, it shows today's component and stays in
   the round as `0-current.html` (Option 0, for comparison); for a new component it shows where the
   component will sit. Get the baseline right first: it is the context every variant is judged in.
2. **Variant pages.** Copy the baseline once per variant to `{n}-{slug}.html` and replace only the
   component's region and its inline styles. Each page looks like the real app: no option labels,
   badges or explanations on it; the `<title>` carries the option number and name.
3. **Overview.** `index.html` has a page header (what is explored, what stays fixed), a jump link per
   option, and per option: the number badge, short name and one-line description, an
   **Open full page** link (`target="_blank"`) and the page itself in an `<iframe>` at the desktop
   viewport height. No JavaScript.

**Per variant:**
- Plausible mock data in the codebase's data shape, the same data in every variant
- CSS `:hover` / `:focus` states where relevant
- Other states that change the decision (empty, filtered, error) as extra full pages
  `{n}-{slug}--{state}.html`, linked under the option's frame in the overview

**Variant count:** 7 by default. User can request more or fewer. Use one theme and one viewport unless
the user asks for more.

**Variant quality rules:**
- Each variant must be **structurally different** — different layout composition, information hierarchy, or visual treatment. Not just color/size tweaks.
- Each variant must **fit the app** (see Fit the App).
- Consult the `frontend-design` and `professional-ui-calibration` skills, when available, for quality standards.
- No gradient text, no glassmorphism, no neon-on-dark unless the project's design system uses them.

Open the overview in the browser after generating: `open design/{round}/index.html` (macOS) or
`xdg-open design/{round}/index.html` (Linux).

### Phase 4 — Iterate

When the user gives feedback ("I like the badges from Option 3 and the bar from Option 5"):

1. Create a new round folder from the same baseline: `design/{component-name}-v{N}/`
2. Combine the selected elements into 2-3 refined variants, each a full page (keep `0-current.html`
   when redesigning)
3. Show all relevant states for each refined variant as state pages
4. Open its overview in the browser

Repeat until the user picks a final direction.

### Phase 5 — Implement

When the user says "implement this" or picks a final option, the repository's engineering rules (tests,
checks, tickets, reviews) apply from here:
1. Implement the chosen design in the actual codebase
2. Reuse existing project components and primitives where possible
3. Extend primitives if needed to keep the project maintainable
4. Optionally clean up demo HTML files (ask the user)

## Demo Page Quality Checklist

See [references/demo-page-guidelines.md](references/demo-page-guidelines.md) for the round's file layout, page and overview HTML patterns, variant differentiation strategies, and common pitfalls.

## Key Principles

- Demo pages are **disposable artifacts** for the design conversation
- Always use the project's **real design tokens**, never generic values
- Variants must be **structurally different**, not superficial tweaks
- Every variant is **a full app page** that fits the product's design, compared in one overview and
  openable on its own
- Use **plausible data** in the codebase's data shape
- **Hand-written static HTML + CSS** — no JavaScript, no build step, no generator scripts
