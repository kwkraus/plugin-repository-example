# MAF Migration Assistant Plugin

This plugin packages the repository's Microsoft Agent Framework migration assets into a single self-contained folder for manual copying into a private marketplace.

## Included assets

- `agents/maf-migrate-csharp.agent.md`
- `agents/maf-migrate-python.agent.md`
- `skills/maf-migration-kb/SKILL.md`
- `skills/maf-fetch-docs/SKILL.md`

## Notes

- `plugin.json` is the manifest entry point for the package.
- The agents and skills inside this folder are packaged copies of the source assets under `.github/`.
- If you update the source assets later, keep this packaged copy in sync before republishing.
