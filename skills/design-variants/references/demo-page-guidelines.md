# Demo Page Guidelines

## HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{Component} — Design Options</title>
<style>
  /* Load project font */
  @import url('https://fonts.googleapis.com/css2?family={Font}:wght@400;500;600;700&display=swap');

  :root {
    /* Mirror project tokens exactly */
    --font-family: '{Font}', system fallbacks;
    --gray-50: #...;
    /* ... all relevant tokens */
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: var(--font-family);
    font-size: 14px;
    background: var(--surface-canvas);
    -webkit-font-smoothing: antialiased;
    padding: 48px 24px 100px;
  }

  /* Page layout */
  .page-header { max-width: 960px; margin: 0 auto 56px; }
  .option-wrapper { max-width: 960px; margin: 0 auto 64px; }

  /* Option label with number badge */
  .option-tag { ... }
  .option-tag .num { /* small pill badge */ }
  .option-tag h2 { /* option name */ }
  .option-tag .hint { /* one-line description, right-aligned */ }

  /* Shared component styles matching project */
  /* ... */

  /* Per-option unique styles prefixed: .opt1-, .opt2-, etc. */
  /* ... */
</style>
</head>
<body>
  <div class="page-header">
    <h1>{Component} — Design Options</h1>
    <p>Short description of what's being explored.</p>
  </div>

  <!-- Each option -->
  <div class="option-wrapper">
    <div class="option-tag">
      <span class="num">Option 1</span>
      <h2>Short Name</h2>
      <span class="hint">One-line description of approach</span>
    </div>
    <!-- Component mockup -->
  </div>
</body>
</html>
```

## Variant Differentiation Strategies

Each variant must differ in at least one structural dimension:

- **Information hierarchy** — what leads, what's secondary, what's hidden
- **Layout composition** — sidebar vs stacked, horizontal vs vertical, grid vs list
- **Density** — compact data-heavy vs spacious editorial
- **Component type** — cards vs table vs list vs segmented bar vs inline stats
- **Interaction model** — toggles vs checkboxes vs clickable chips vs segmented control
- **Visual weight distribution** — where the eye goes first

Bad differentiation (avoid): same layout with different border-radius, color shifts, or font size tweaks.

## Showing Multiple States

When a component has meaningful states, show them as separate labeled cards within the same option:

```html
<div class="option-wrapper">
  <div class="option-tag">...</div>

  <!-- Default state -->
  <div class="card">...</div>

  <!-- Labeled state -->
  <div class="state-label">With filters applied</div>
  <div class="card">...</div>
</div>
```

Common states to consider:
- Default / populated
- Empty / no data
- Loading
- With filters applied
- Error
- Single item vs many items

## Iteration Pages

When refining after user feedback, create `{component}-design-v{N}.html`:

- Reduce to 2-3 variants combining elements the user liked
- Show all relevant states for each variant
- Keep the same token/style foundation — only the variants change

## Common Pitfalls

- **Generic tokens** — never use `#333`, `16px`, `8px` directly. Always reference the project's actual token values.
- **Isolated fragments** — a button floating in space tells nothing. Show the component in its full context (card, section, page).
- **Same structure, different paint** — if you squint and all options look the same, they're not different enough.
- **Missing data realism** — use data shapes from the actual codebase (read the types/models). "Item 1, Item 2" is not realistic.
- **Overloaded pages** — 7 full-section variants can be long. Keep each variant focused on the key differences, not a full page rebuild.
