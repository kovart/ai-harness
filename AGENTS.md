# Repository Harness

Keep this repository small, public-safe, and compatible with the Agent Skills ecosystem.

## Rules

- Put each skill directly under `skills/<skill-name>/`; never repeat the skill name in a nested folder.
- Keep `SKILL.md` frontmatter limited to `name` and `description`, and make `name` match the directory.
- Prefer concise procedural instructions. Add `references/`, `scripts/`, or `assets/` only when the skill uses them.
- Keep tool-specific UI metadata under `agents/`; keep the main skill usable across agents.
- Do not commit credentials, private URLs, personal notes, organization-only instructions, or machine-specific paths.
- Do not copy private repositories or configuration wholesale. Publish only deliberately reviewed files.
- Make the smallest change that solves the current need; do not scaffold speculative categories.

## Validation

Run before every commit:

```bash
npx -y skills@1.5.18 add . --list
```
