# Demo Page Guidelines

## Round Layout

```text
design/{component-name}-system/       design:export output, when the project has one
design/{component-name}-options/      first round; iterations are {component-name}-v{N}/
  index.html                          overview: every option framed in the full app
  tokens.css                          token mirror, only without an export
  page.css                            shell, page header and neighbouring blocks, shared by every page
  0-current.html                      today's page, when redesigning
  1-{slug}.html … 7-{slug}.html       one full app page per variant
  3-{slug}--empty.html                extra states, only where they change the decision
```

Build `0-current.html` (or the baseline for a new component) first, then copy it per variant and edit
only the component's region. Shell and neighbour markup is duplicated across pages on purpose: each
page must open on its own.

## Variant Page

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Option 3 · Inline Stats — {Component}</title>
<!-- With an export: ../{component-name}-system/tokens.css and styles.css. Without: tokens.css -->
<link rel="stylesheet" href="../{component-name}-system/tokens.css">
<link rel="stylesheet" href="../{component-name}-system/styles.css">
<link rel="stylesheet" href="page.css">
<style>
  /* Only this variant's component styles, prefixed .v3- */
</style>
</head>
<body>
  <!-- Shell, top bar, page header: identical in every page of the round -->
  <!-- Neighbouring blocks above -->

  <!-- ▼ Option 3 -->
  <section class="v3-summary">...</section>
  <!-- ▲ Option 3 -->

  <!-- Neighbouring blocks below -->
</body>
</html>
```

The page fills the viewport the way the app does (fixed shell, fluid content), so it renders correctly
both inside the overview frame and as a full page. No option labels or notes on it.

## Overview

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{Component} — Design Options</title>
<!-- Tokens only (the export's tokens.css or the round's tokens.css); never page.css -->
<link rel="stylesheet" href="../{component-name}-system/tokens.css">
<style>
  body { margin: 0; padding: 32px 24px 96px; background: var(--surface-canvas); }
  .overview-header, .option { max-width: 1440px; margin-inline: auto; }
  .overview-header { margin-bottom: 40px; }
  .jump { display: flex; flex-wrap: wrap; gap: 8px 16px; }
  .option { margin-bottom: 64px; scroll-margin-top: 24px; }
  .option-tag { display: flex; align-items: baseline; gap: 12px; margin-bottom: 12px; }
  .option-tag .hint { flex: 1; }
  .frame { display: block; width: 100%; height: 900px; border: 1px solid var(--border-subtle);
           border-radius: 12px; }
</style>
</head>
<body>
  <header class="overview-header">
    <h1>{Component} — Design Options</h1>
    <p>What is explored, and what stays fixed across options.</p>
    <nav class="jump">
      <a href="#option-0">0 · Current</a>
      <a href="#option-1">1 · Inline Stats</a>
      <!-- one per option -->
    </nav>
  </header>

  <section class="option" id="option-1">
    <div class="option-tag">
      <span class="num">Option 1</span>
      <h2>Inline Stats</h2>
      <span class="hint">One-line description of the approach</span>
      <a href="1-inline-stats.html" target="_blank">Open full page ↗</a>
    </div>
    <iframe class="frame" src="1-inline-stats.html" title="Option 1 · Inline Stats" loading="lazy"></iframe>
    <p class="states">States:
      <a href="1-inline-stats--empty.html" target="_blank">Empty ↗</a>
    </p>
  </section>
</body>
</html>
```

Map the overview's colors and border to the project's real token names. The frame shows the page at
real size, not scaled down: the user reads it the way the app will be read.

## Variant Differentiation Strategies

Each variant must differ in at least one structural dimension:

- **Information hierarchy** — what leads, what's secondary, what's hidden
- **Layout composition** — sidebar vs stacked, horizontal vs vertical, grid vs list
- **Density** — compact data-heavy vs spacious editorial
- **Component type** — cards vs table vs list vs segmented bar vs inline stats
- **Interaction model** — toggles vs checkboxes vs clickable chips vs segmented control
- **Visual weight distribution** — where the eye goes first

Bad differentiation (avoid): same layout with different border-radius, color shifts, or font size tweaks.

Differences stay within the app's design language: density and weight can vary, but only within the
range the app already uses.

## Showing Multiple States

Each state that changes the design decision is its own full page, `{n}-{slug}--{state}.html`, copied
from the variant page with only the component's state changed, and linked under the option's frame.

Common states to consider:
- Default / populated
- Empty / no data
- Loading
- With filters applied
- Error
- Single item vs many items

## Iteration Rounds

When refining after user feedback, create `design/{component-name}-v{N}/` in the same Git-ignored artifact folder:

- Reduce to 2-3 variants combining elements the user liked
- Show all relevant states for each variant
- Reuse the same baseline, tokens and page styles — only the variants change

## Common Pitfalls

- **Generic tokens** — never use `#333`, `16px`, `8px` directly. Always reference the project's actual token values.
- **Isolated fragments** — a component floating on a blank page tells nothing. Every variant sits in the full app page.
- **Foreign variants** — a variant with its own type scale, radii, shadows or density looks like a different product next to the shell. Rework it until it belongs.
- **Drifting context** — shell, neighbours or data that differ between variants make the comparison unfair. Copy the baseline; edit only the component.
- **Wireframe context** — a grey-box sidebar or placeholder neighbours make every variant look out of place. Draw the context at the same fidelity as the variant.
- **Labels inside the app** — option badges or notes on a variant page break the full-page view. They belong in the overview.
- **Same structure, different paint** — if you squint and all options look the same, they're not different enough.
- **Missing data realism** — use data shapes from the actual codebase (read the types/models). "Item 1, Item 2" is not realistic.
- **Engineering the mockup** — generator scripts, React/Vite harnesses, token extraction from the running app, or screenshot/measure/diff pipelines. The artifact is hand-written HTML + CSS; spend the effort on pixels.
