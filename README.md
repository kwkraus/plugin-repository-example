# Plugin Marketplace Example

A curated marketplace for GitHub Copilot plugins used in demos and examples.

## Overview

This repository hosts a GitHub Copilot CLI plugin marketplace with plugins designed for demonstration and educational purposes. It follows the official GitHub Copilot plugin marketplace patterns.

## Quick Start

### Adding This Marketplace to Copilot CLI

To add this marketplace to your Copilot CLI installation, run:

```bash
copilot plugin marketplace add <owner>/<repo>
```

For example:
```bash
copilot plugin marketplace add kwkraus/plugin-repository-example
```

### Installing Plugins

Once the marketplace is registered, you can install plugins using:

```bash
copilot plugin install <plugin-name>@plugin-marketplace-example
```

## Available Plugins

Plugins will be added to this marketplace over time. Check back soon for updates!

| Plugin Name | Description | Version |
|-----------|-----------|---------|
| (Coming soon) | Placeholder for future plugins | — |

## About This Marketplace

- **Name**: plugin-marketplace-example
- **Owner**: Kevin Kraus
- **Description**: Marketplace for plugins I use for my demos

## Marketplace Structure

```
.
├── .github/
│   └── plugin/
│       └── marketplace.json    # Marketplace metadata
├── plugins/                    # Plugin directories
│   └── (individual plugin folders)
└── README.md                   # This file
```

## Adding Plugins

To add a new plugin to this marketplace:

1. Create a plugin directory in the `plugins/` folder
2. Follow GitHub's plugin creation guidelines
3. Update `marketplace.json` with the plugin metadata
4. See [CONTRIBUTING.md](./CONTRIBUTING.md) for detailed guidelines

## Resources

- [GitHub Copilot CLI Documentation](https://docs.github.com/en/copilot/how-tos/copilot-cli)
- [Creating Plugins for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating)
- [Plugin Marketplace Guide](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-marketplace)

## License

This marketplace and its contents are subject to the applicable licenses. Please see individual plugin directories for specific license information.
