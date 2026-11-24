# Tability plugins for Claude

This repository is a Claude Code plugin marketplace. It contains one plugin, [`tability`](plugins/tability), which connects Claude to [Tability](https://www.tability.io) and adds skills for getting started, OKR tracking, status reports, SMART goals, KPI reporting and strategy alignment.

## Install in Claude Code

In a Claude Code session:

```
/plugin marketplace add tabilityio/tability-claude-plugin
/plugin install tability@tability
```

Or from a terminal:

```bash
claude plugin marketplace add tabilityio/tability-claude-plugin
```

```bash
claude plugin install tability@tability
```

The first time Claude calls a Tability tool, it opens a browser window so you can sign in to Tability (OAuth). No API key is needed.

The plugin connects to Tability's US server. For workspaces hosted in the EU or Australia, contact support@tability.io.

See the [plugin README](plugins/tability/README.md) for the skills and example requests.

## Repository layout

```
.claude-plugin/marketplace.json     Marketplace catalog
plugins/tability/
  .claude-plugin/plugin.json        Plugin manifest
  .mcp.json                         Tability MCP server (https://mcp.tability.app/mcp)
  skills/<skill>/SKILL.md           Six workflow skills
  evals/<case>/                     Skill-trigger test cases for `claude plugin eval`
```

## Development

Validate before committing:

```bash
claude plugin validate ./plugins/tability
```

```bash
claude plugin validate .
```

Test a local checkout:

```bash
claude plugin marketplace add ./
```

Run the trigger tests (uses real model calls):

```bash
claude plugin eval ./plugins/tability
```
