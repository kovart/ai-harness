# Repository Harness

Keep this repository small, portable, and compatible with the Agent Skills ecosystem.

## Rules

- Put each skill directly under `skills/<skill-name>/`; never repeat the skill name in a nested folder.
- Keep `SKILL.md` frontmatter limited to `name` and `description`, and make `name` match the directory.
- Prefer concise procedural instructions. Add `references/`, `scripts/`, or `assets/` only when the skill uses them.
- Keep tool-specific UI metadata under `agents/`; keep the main skill usable across agents.
- Do not commit credentials, personal data, environment-specific URLs, or machine-specific paths.
- Make the smallest change that solves the current need; do not scaffold speculative categories.

## Validation

Run before every commit:

```bash
npx -y skills@1.5.18 add . --list
```
