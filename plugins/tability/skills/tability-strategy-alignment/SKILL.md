---
name: tability-strategy-alignment
description: Map business goals to strategy, cascade goals from company to team to individual, and map dependencies between goals and initiatives in Tability. Links outcomes that contribute to higher-level goals, finds orphaned or misaligned goals, checks that every team goal supports the company strategy, and charts which initiatives depend on which outcomes. Use this whenever someone wants cascading goals, goal alignment, strategy alignment, business goals mapping, a goal tree or strategy map, OKR alignment across teams, dependency mapping between goals, projects or initiatives, or asks "how does my team's work ladder up", even if they don't mention Tability.
compatibility: Requires the Tability MCP connector (tools named tability_*).
---

# Strategy alignment, cascading goals and dependencies in Tability

Help the user see and fix how goals connect: strategy at the top, company outcomes below it, team and individual outcomes contributing to those, and initiatives doing the work.

## Read the current structure

1. Confirm workspace and period. For the workspace: call `tability_list_workspaces` to see the workspaces this connection can use; pick the only one, or ask which. If that tool isn't available, ask for the workspace slug: the part after `tability.app/` in the user's Tability URL. A `403 insufficient_scope` error means this connection wasn't authorized for it (see `tability-getting-started`). Then `tability_search_plans` (company and team plans for the period), then `tability_get_plan_content` for each. Skip sample plans (`is_dummy: true`) unless asked.
2. Reporting lines help suggest cascades: `tability_get_membership_direct_reports` and `tability_get_membership_manager`.
3. Build the picture from what the connector exposes:
   - **Plan hierarchy:** each plan's `parent_id` shows which team plans sit under which company plan.
   - **Initiative to outcome:** initiatives are listed under their outcome in plan content, or use `tability_search_initiatives` with `outcome_id`.
   - **Goal to goal links can't be listed.** The connector can create `contributes_to` and `relates_to` links but can't read them back. Each outcome's `outcome_contributors_count` says how many outcomes contribute to it, but not which ones.

## Alignment audit

Report, with names and owners:
- **Unsupported goals**: company outcomes with `outcome_contributors_count: 0`. These are at risk because nothing is recorded as working toward them.
- **Orphans**: team or individual outcomes that contribute to nothing above them. You can only be sure when every outcome in the parent plan has 0 contributors. Otherwise, say which parent outcomes already have contributors (and how many), explain that the links themselves aren't visible through Claude, and point the user to the alignment view in Tability to check which outcomes they are.
- **Overloaded goals**: outcomes with many contributors, or one person owning contributors to many unrelated goals.
- **Initiatives with no outcome**: work not tied to any goal.

Keep judgments modest: an orphan may be legitimate local work. Ask rather than assume.

## Cascading goals

When cascading a company goal down:
1. Draft, for each team, one or two outcomes that would move the company outcome, in that team's language and metrics. A cascade is not copying the parent goal; each child should be something the team can actually move.
2. Get approval, then create with `tability_create_outcome` in the team's plan.
3. Link each child to its parent with `tability_upsert_relationship`, `relationship_type: "contributes_to"`, source = child, target = parent. Use `relates_to` for associations without a direction.

## Dependency mapping

For "what depends on what": list initiatives per outcome (`tability_search_initiatives`), and ask about cross-team dependencies not recorded in Tability. Show the result as a diagram (goal tree or dependency graph) when the environment supports one, otherwise as an indented outline. Mark critical paths: initiatives that several outcomes rely on, and outcomes whose only initiative is off track.

## Avoid

- Don't create or change relationships without showing the proposed links first; `tability_upsert_relationship` replaces an existing link of a different type and removes reverse links.
- Don't restructure plans or reassign owners unless asked.
- Don't present a goal tree as complete when some outcomes have contributors you can't see. Say what's missing.
