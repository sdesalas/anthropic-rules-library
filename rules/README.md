# Anthropic Rules Library

Detection rules for Elastic Security that watch AI coding agents, MCP servers and Claude usage: Claude Code running
commands it should not, credentials and data leaving through the agent, new MCP servers, Anthropic organization
changes and Claude models on Amazon Bedrock. The rules are mapped to [MITRE ATT&CK](https://attack.mitre.org/) and
[MITRE ATLAS](https://atlas.mitre.org/).

Each file holds one rule. Rules are grouped by platform in the `windows`, `linux`, `macos` and `cross-platform`
folders, and by data source under `integrations`.

The rules are sample content, written for demonstration. Review and test them before turning them on.
