# Contributing

## Add or update a skill

1. Create or edit `skills/<skill-name>/SKILL.md`.
2. Keep the skill directory flat: `skills/<skill-name>/SKILL.md`.
3. Add only the references, scripts, assets, or agent metadata the skill uses.
4. Check every file for credentials, personal data, and environment-specific assumptions.
5. Run:

   ```bash
   npx -y skills@1.5.18 add . --list
   ```

6. Open a focused pull request explaining the behavior the skill adds or changes.

A change is ready when the skill is discoverable, all local links resolve, and no sensitive material is included.
