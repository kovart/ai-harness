# AI Harness

Battle-tested agent skills and the lightweight harness used to maintain them.
This repository is intentionally public and contains only reusable, shareable material.

## Install

List the available skills:

```bash
npx skills add kovart/ai-harness --list
```

Install `design-variants` globally for Codex and Claude Code:

```bash
npx skills add kovart/ai-harness --skill design-variants -g --agent codex --agent claude-code -y
```

## Repository layout

- `skills/<name>/` — one flat directory per reusable skill.
- `AGENTS.md` — the maintenance harness followed by coding agents in this repository.
- `.github/workflows/validate.yml` — verifies that the skills CLI can discover every skill.

Each skill owns its instructions, optional agent metadata, and only the references or assets it actually needs.

## Available skills

- [`design-variants`](skills/design-variants/SKILL.md) — generate structurally different HTML design options using a project's real design tokens.

See [CONTRIBUTING.md](CONTRIBUTING.md) before adding or changing a skill.

## License

[MIT](LICENSE)
