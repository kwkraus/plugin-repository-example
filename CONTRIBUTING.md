# Contributing to Plugin Marketplace Example

Thank you for your interest in contributing to this GitHub Copilot plugin marketplace!

## Adding a Plugin

To contribute a new plugin to this marketplace, follow these steps:

### 1. Create Your Plugin

First, create a plugin following the official GitHub guidelines:
- [Creating a plugin for GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating)

### 2. Add to This Repository

1. Create a new directory under `plugins/` with your plugin name
2. Add your plugin files to that directory
3. Ensure your plugin includes:
   - A valid `manifest.json` file
   - Clear documentation in a `README.md`
   - Any required source files and assets

### 3. Update marketplace.json

Add your plugin to `.github/plugin/marketplace.json` with the following information:

```json
{
  "name": "your-plugin-name",
  "description": "Brief description of what your plugin does",
  "version": "1.0.0",
  "source": "./plugins/your-plugin-name"
}
```

### 4. Create a Pull Request

1. Fork this repository
2. Create a feature branch: `git checkout -b add/your-plugin-name`
3. Commit your changes: `git commit -m "Add your-plugin-name plugin"`
4. Push to your fork: `git push origin add/your-plugin-name`
5. Open a pull request with a clear description

## Plugin Guidelines

- **Clarity**: Ensure your plugin's purpose is clear and well-documented
- **Quality**: Test your plugin thoroughly before submission
- **Documentation**: Include a comprehensive README.md in your plugin directory
- **Versioning**: Follow [Semantic Versioning](https://semver.org/) for your plugin versions
- **License**: Include appropriate license information

## Plugin Manifest

Each plugin must include a `manifest.json` file. For details on the required fields, see:
- [GitHub Copilot CLI plugin reference](https://docs.github.com/en/copilot/reference/cli-plugin-reference)

## Questions?

If you have questions about creating plugins or contributing, please:
1. Check the [GitHub Copilot CLI documentation](https://docs.github.com/en/copilot/how-tos/copilot-cli)
2. Review existing plugin examples in this marketplace (once available)
3. Open an issue in this repository

## Code of Conduct

Please be respectful and constructive in all interactions. This marketplace is a community resource.
