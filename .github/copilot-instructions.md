# Copilot Instructions for plugin-repository-example

This is a GitHub Copilot CLI **plugin marketplace** repository—a curated collection of plugins for demonstration and educational purposes.

## Code Review Instructions

The following instructions are only to be applied when performing a code review.

### README updates

- [ ] The new file should be added to the `docs/README.<type>.md`.

### Prompt file guide

**Only apply to files that end in `.prompt.md`**

- [ ] The prompt has markdown front matter.
- [ ] The prompt has a `agent` field specified of either `agent`, `ask`, or `Plan`.
- [ ] The prompt has a `description` field.
- [ ] The `description` field is not empty.
- [ ] The file name is lower case, with words separated by hyphens.
- [ ] Encourage the use of `tools`, but it's not required.
- [ ] Strongly encourage the use of `model` to specify the model that the prompt is optimised for.
- [ ] Strongly encourage the use of `name` to set the name for the prompt.

### Instruction file guide

**Only apply to files that end in `.instructions.md`**

- [ ] The instruction has markdown front matter.
- [ ] The instruction has a `description` field.
- [ ] The `description` field is not empty.
- [ ] The file name is lower case, with words separated by hyphens.
- [ ] The instruction has an `applyTo` field that specifies the file or files to which the instructions apply. If they wish to specify multiple file paths they should formatted like `'**.js, **.ts'`.

### Agent file guide

**Only apply to files that end in `.agent.md`**

- [ ] The agent has markdown front matter.
- [ ] The agent has a `description` field.
- [ ] The `description` field is not empty.
- [ ] The file name is lower case, with words separated by hyphens.
- [ ] Encourage the use of `tools`, but it's not required.
- [ ] Strongly encourage the use of `model` to specify the model that the agent is optimised for.
- [ ] Strongly encourage the use of `name` to set the name for the agent.

### Agent Skills guide

**Only apply to folders in the `skills/` directory**

- [ ] The skill folder contains a `SKILL.md` file.
- [ ] The SKILL.md has markdown front matter.
- [ ] The SKILL.md has a `name` field.
- [ ] The `name` field value is lowercase with words separated by hyphens.
- [ ] The `name` field matches the folder name.
- [ ] The SKILL.md has a `description` field.
- [ ] The `description` field is not empty, at least 10 characters, and maximum 1024 characters.
- [ ] The `description` field value is wrapped in single quotes.
- [ ] The folder name is lower case, with words separated by hyphens.
- [ ] Any bundled assets (scripts, templates, data files) are referenced in the SKILL.md instructions.
- [ ] Bundled assets are reasonably sized (under 5MB per file).

### Plugin guide

**Only apply to directories in the `plugins/` directory**

- [ ] The plugin directory contains a `.github/plugin/plugin.json` file.
- [ ] The plugin directory contains a `README.md` file.
- [ ] The plugin.json has a `name` field matching the directory name.
- [ ] The plugin.json has a `description` field.
- [ ] The `description` field is not empty.
- [ ] The directory name is lower case, with words separated by hyphens.
- [ ] If `tags` is present, it is an array of lowercase hyphenated strings.
- [ ] If `items` is present, each item has `path` and `kind` fields.
- [ ] The `kind` field value is one of: `prompt`, `agent`, `instruction`, `skill`, or `hook`.
- [ ] The plugin does not reference non-existent files.

## Repository Purpose

This marketplace allows users to discover and install Copilot CLI plugins. The marketplace is registered with Copilot CLI via:
```bash
copilot plugin marketplace add kwkraus/plugin-repository-example
```

## Key Architecture

The repository has a simple, flat structure:

- **`.github/plugin/marketplace.json`** – The canonical marketplace metadata. Contains:
  - Marketplace name, owner, and description
  - `plugins` array: each plugin entry must have `name`, `description`, `version`, and `source` (path to plugin directory)
  - This file is the source of truth for what plugins are available

- **`plugins/` directory** – Contains individual plugin subdirectories. Each plugin should follow the official GitHub Copilot plugin structure:
  - `manifest.json` – Required plugin metadata (see GitHub's [plugin reference](https://docs.github.com/en/copilot/reference/cli-plugin-reference))
  - `README.md` – Plugin-specific documentation
  - Source files and assets as needed

- **`README.md`** – User-facing overview (describes the marketplace, not individual plugins)
- **`CONTRIBUTING.md`** – Guidelines for adding new plugins to this marketplace

## Core Workflows

### Adding a New Plugin

1. Create a directory in `plugins/` named after the plugin (e.g., `plugins/my-plugin`)
2. Create `manifest.json` in the plugin directory (required fields per [GitHub docs](https://docs.github.com/en/copilot/reference/cli-plugin-reference))
3. Add `README.md` documenting the plugin's purpose and usage
4. Add the plugin entry to `.github/plugin/marketplace.json` with:
   ```json
   {
     "name": "my-plugin",
     "description": "What the plugin does",
     "version": "1.0.0",
     "source": "./plugins/my-plugin"
   }
   ```
5. Include any required source files, configurations, or assets in the plugin directory

### Verifying Plugin Structure

Check that each plugin in `marketplace.json`:
- Has a corresponding directory in `plugins/`
- Contains a valid `manifest.json`
- Has a `README.md` with clear documentation
- Follows [Semantic Versioning](https://semver.org/)

## No Build or Test Pipeline

This repository does **not** have automated build, test, or lint commands. Plugin validation is manual and follows the guidelines in CONTRIBUTING.md. Each plugin is responsible for its own validation according to GitHub's plugin documentation.

## Key Conventions

- **Versioning**: Use Semantic Versioning (e.g., 1.0.0, 1.1.0, 2.0.0-beta) for plugin versions
- **Plugin naming**: Use kebab-case for plugin directory names and in `manifest.json` (e.g., `my-awesome-plugin`)
- **Documentation**: Every plugin must have a comprehensive `README.md` in its directory
- **Plugin source path**: Always use relative paths in `marketplace.json` (`./plugins/plugin-name`)

## Contributing Process

Contributors should follow the process in CONTRIBUTING.md:
1. Fork the repository
2. Create a feature branch: `git checkout -b add/plugin-name`
3. Add plugin files and update `marketplace.json`
4. Open a pull request with clear description

See CONTRIBUTING.md for the complete contribution guidelines, including plugin quality standards.

## Resources

- [GitHub Copilot CLI Documentation](https://docs.github.com/en/copilot/how-tos/copilot-cli)
- [Creating Plugins for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating)
- [Plugin Marketplace Guide](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-marketplace)
- [Plugin Reference](https://docs.github.com/en/copilot/reference/cli-plugin-reference)
