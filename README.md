<h1><img src="anthropic-logo.svg" width="32" height="32"/>  Anthropic Rules Library</h1>

A detection rule library in git, with 21 sample rules about AI coding agents, MCP servers and Claude usage. It shows a
third rule source for Elastic Security, next to the prebuilt rules and
[`community-rules-library`](https://github.com/sdesalas/community-rules-library), delivered the same way as
[`detection-rules-library`](https://github.com/sdesalas/detection-rules-library): as a git repository instead of a
Fleet package.

The rules are sample content for demonstration. They are not tested for production use, and the library is not
published or endorsed by Anthropic.

## Layout

```
manifest.yml                         package manifest (name, version, kibana.version condition)
changelog.yml                        one entry per release
anthropic-logo.svg                   the package icon (the manifest's `icons`)
LICENSE.txt                          Elastic License 2.0 (`source.license: Elastic-2.0`)
rules/
  README.md                          the package description
  windows/<rule file name>.yml       one file per rule
  linux/...
  macos/...
  cross-platform/...
  integrations/anthropic/...         rules for a data source
  integrations/aws/...
```

- Every folder under `rules/` holds rule files, and only those. The folder a rule is in says where it belongs.
- The package files are at the root, so the manifest, changelog and license also cover exception lists and value lists when they are added.
- One file per rule. Its git history is its list of versions, so don't add a file per version.

## Rule files

Each file holds one rule as `{ id, type: "security-rule", attributes }`, in YAML. `id` is `<rule_id>_<version>`, and
`attributes.rule_id` and `attributes.version` are the source of truth for identity and version, not the file name.

The files follow the same conventions as the YAML files in `detection-rules-library`: keys in sorted order, multi-line
fields as literal blocks, one document per file, no anchors or aliases. The file name is the rule's name in snake case,
prefixed with its tactic.

Rules map to both [MITRE ATT&CK](https://attack.mitre.org/) and [MITRE ATLAS](https://atlas.mitre.org/), and are
tagged `Domain: GenAI` and `Source: Anthropic Rules Library`. The endpoint rules use Elastic Defend data. Claude Code
runs shell commands through a shell that sources a snapshot under `~/.claude/shell-snapshots`, so most of them match
both processes started by `claude` and processes started by that shell.

| Rule | Folder | Type | Severity |
|---|---|---|---|
| Encoded PowerShell Command Run by Claude Code | `windows` | eql | high |
| Registry Run Key or Scheduled Task Added by Claude Code | `windows` | eql | medium |
| Security Tooling Disabled by Claude Code | `windows` | eql | high |
| Reverse Shell Started by Claude Code | `linux` | eql | critical |
| Systemd Unit Created by Claude Code | `linux` | eql | medium |
| First Sudo Use by Claude Code on a Host | `linux` | new_terms | low |
| Claude Desktop Extension Downloaded from a Browser | `macos` | eql | low |
| Suspicious AppleScript Run by Claude Code | `macos` | eql | high |
| Keychain Secrets Read by Claude Code | `macos` | eql | high |
| Claude Code Project Settings Written by Git | `cross-platform` | eql | low |
| Headless Claude Code Started by a Scheduler or Server | `cross-platform` | new_terms | medium |
| Credential File Read by Claude Code | `cross-platform` | eql | high |
| Data Sent to a File Sharing or Request Capture Site by Claude Code | `cross-platform` | eql | high |
| Anthropic API Contacted by a New Process | `cross-platform` | new_terms | medium |
| Anthropic API Key in a Process Command Line | `cross-platform` | eql | medium |
| New MCP Server Command Started by Claude | `cross-platform` | new_terms | low |
| Burst of Discovery Commands Run by Claude Code | `cross-platform` | esql | medium |
| Multiple Anthropic Organization Invites Sent by One User | `integrations/anthropic` | threshold | medium |
| Anthropic Admin API Key Created and Deleted in a Short Window | `integrations/anthropic` | esql | medium |
| Claude Model on Bedrock Invoked by a New IAM Identity | `integrations/aws` | new_terms | low |
| Claude Model on Bedrock Invoked with a Long-Term Access Key | `integrations/aws` | query | medium |

## Releases

The `main` branch holds the latest release. Each release is one commit with a tag `main/v<package version>`, for
example `main/v0.0.1`, so a consumer can map a package version to a commit. A consumer can list tags to find
releases, compare the `rules/` trees of two commits to see what changed, and fetch single files without cloning.

## Using it with Kibana

Nothing in Kibana reads this repo by default. It is used to test delivering rules from a git provider, alongside the
prebuilt rules library.
