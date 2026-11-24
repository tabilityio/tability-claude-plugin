# Tability plugin

OKR tracking, goal tracking and KPI reporting in Claude, powered by [Tability](https://www.tability.io). The plugin connects Claude to your Tability workspace and teaches it to run the whole goal-setting cycle well: drafting OKRs and SMART goals, weekly check-ins, status reports, KPI scorecards, cascading goals and mapping dependencies between teams.

## What's included

- **Tability MCP connector** (`https://mcp.tability.app/mcp`) for reading and writing plans, objectives, outcomes (key results), initiatives, check-ins and KPIs.
- **Six skills** that Claude loads automatically when a request matches:

| Skill | Example request |
| --- | --- |
| `tability-getting-started` | "I just installed the Tability plugin. Help me get started." |
| `tability-okr-tracking` | "Help me set up our Q1 OKRs for the product team and track them" |
| `tability-status-report` | "Write this week's status report for leadership on our goals" |
| `tability-smart-goals` | "Turn 'get better at customer retention' into a SMART goal and track it" |
| `tability-kpi-reporting` | "Here are October's revenue, churn and NPS numbers, update our KPIs and summarize" |
| `tability-strategy-alignment` | "Cascade our company goal of 20% ARR growth down to sales and marketing" |

You can also run a skill directly, for example `/tability:tability-status-report`.

## Data region

This plugin connects to Tability's US server (`mcp.tability.app`). If your workspace is hosted in the EU or Australia, contact [support@tability.io](mailto:support@tability.io) to connect Claude to your region.

## Signing in

The first time Claude calls a Tability tool, it opens a browser window for you to sign in to Tability with OAuth. No API key is stored in the plugin. If you belong to several workspaces, Claude asks which one to use.

New to Tability? Ask Claude to "help me get started with Tability". It links you to a free trial, then helps you set up your first plan. You can also [sign up directly](https://tability.app/signup?utm_source=claude&utm_medium=plugin&utm_campaign=readme).

## Before it writes anything

Claude drafts objectives, key results, check-ins and comments in chat and asks for your approval before creating them in Tability.

**Check-ins notify your teammates.** Posting a check-in can send Tability notifications, Slack messages and webhooks, and these can't be recalled. Try the plugin in a test workspace first if you want to experiment.

## Install

```
/plugin marketplace add tabilityio/tability-claude-plugin
/plugin install tability@tability
```
