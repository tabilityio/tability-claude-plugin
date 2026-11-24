---
name: tability-status-report
description: Write internal status reports and progress updates from goal and OKR data in Tability. Pulls the latest check-ins, key result progress and initiative comments for a plan, team or owner, flags what is off track, stale or unowned, and drafts a weekly, monthly or quarterly update for leadership, the board or the whole company. Use this whenever someone asks for a status report, progress update, weekly update, goals review, OKR review, exec summary, all-hands update or "how are we doing on our goals", even if they don't mention Tability.
compatibility: Requires the Tability MCP connector (tools named tability_*).
---

# Internal status reports from Tability

Produce an accurate, skimmable status report grounded in what's actually recorded in Tability. The value of this report is that it's trustworthy, so every claim should trace back to a check-in, comment or metric.

## Gather the data

1. Confirm scope: which workspace, which plan or team or owner, which period, and who the audience is. Ask only for what isn't clear from the request. For the workspace: call `tability_list_workspaces` to see the workspaces this connection can use; pick the only one, or ask which. If that tool isn't available, ask for the workspace slug: the part after `tability.app/` in the user's Tability URL. A `403 insufficient_scope` error means this connection wasn't authorized for that workspace (see `tability-getting-started`).
2. `tability_search_plans`, then `tability_get_plan_content` for the plan's objectives, outcomes and initiatives. Plan content already includes each outcome's latest two check-ins (score, confidence, note and date), which is enough for current status and trend.
3. Use `tability_list_checkins_for_outcome` only when you need a longer history for an outcome.
4. For initiatives, `tability_list_initiative_comments` for recent status.
5. Use `tability_search_outcomes` filters to find problems quickly: `confidence: "red"` or `"yellow"`, `with_pending_checkin: true` (stale), `without_owner: true`. Always add `plan_id` (or `membership_id`) and `archived: false`; a workspace-wide search can return more data than fits in one response.
6. Leave out sample plans and outcomes (`is_dummy: true`, owners marked "(sample)") unless the user asks for them. If you include them, label them as sample data.

## Write the report

Default structure (adapt to the audience):

- **Headline**: one or two sentences on overall health.
- **On track**: brief, grouped by objective.
- **Needs attention**: each at-risk or off-track key result with current vs. target, the owner, the blocker in their words, and the next step or decision needed.
- **Missing data**: outcomes with no recent check-in or no owner. Report these plainly; don't guess their status.
- **Decisions or help needed**: what the reader can act on.

Guidelines:
- Lead with what changed since the last period, not a full inventory.
- Quote numbers exactly as recorded and say when the last check-in was.
- Executive audiences want five or fewer items needing attention; teams want more detail.
- Keep the report in chat unless the user asks for a document or file.

## Optional: write back

If the user wants the summary posted to Tability, offer `tability_create_initiative_comment` on the relevant initiative, or draft check-ins for stale outcomes. Show the text first and get approval: posts notify people and can't be recalled.
