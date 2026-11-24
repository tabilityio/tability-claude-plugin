---
name: tability-smart-goals
description: Turn vague goals into SMART goals (Specific, Measurable, Achievable, Relevant, Time-bound) and track them in Tability. Coaches the user through each SMART element, proposes a measurable target and deadline, then saves the goal as a tracked outcome with a start value, target and owner so progress can be checked in over time. Use this whenever someone wants to write SMART goals, set personal, team or department goals, make a goal measurable, track individual or employee goals, or set goals for a performance cycle, even if they don't mention Tability or OKRs.
compatibility: Requires the Tability MCP connector (tools named tability_*).
---

# SMART goals tracked in Tability

Help the user turn a fuzzy wish into one to three SMART goals, then make them trackable in Tability.

## Coach the goal

Work through SMART in conversation, one gap at a time rather than as a form:

- **Specific**: what exactly changes, for whom? "Get better at sales" becomes "Increase qualified demos booked by the SMB team."
- **Measurable**: what number shows it? Find the current value (baseline) and the target. If there's no number, suggest a proxy or a milestone.
- **Achievable**: sanity-check the jump from baseline to target against the time available. Say so if it looks like a stretch; the user decides.
- **Relevant**: which team or company goal does it support? This makes alignment possible later.
- **Time-bound**: an end date.

Present the final goal as one sentence plus a short summary: baseline, target, deadline, owner, what it supports. Get approval before saving.

## Save to Tability

1. Workspace, if unclear: call `tability_list_workspaces` to see the workspaces this connection can use; pick the only one, or ask which. If that tool isn't available, ask for the workspace slug: the part after `tability.app/` in the user's Tability URL. A `403 insufficient_scope` error means this connection wasn't authorized for that workspace (see `tability-getting-started`).
2. Find a home: `tability_search_plans` for a plan covering the deadline, and `tability_search_objectives` for a fitting objective. Ignore sample plans (`is_dummy: true`). If none fits, offer to create a plan and objective named after the goal's theme.
3. `tability_create_outcome` with the goal as the title, `outcome_type: "improve_metric"` (or `no_metric` for a milestone), `from` = baseline, `to` = target, a `score_format` matching the unit, and the owner's `membership_id` if they've been named (`tability_list_memberships` to find it).
4. If the user named what it supports, offer to link it with `tability_upsert_relationship` (`contributes_to`).

Tell the user what was created and suggest a check-in cadence. If they want to log progress now, draft the score, confidence and note, then confirm before posting with `tability_create_checkin`, since check-ins send notifications that can't be recalled.

## Avoid

- Don't force SMART onto goals that are deliberately exploratory; say that a milestone or confidence-tracked outcome may fit better.
- Don't record sensitive personal information in goals; keep it to business metrics.
