---
name: tability-okr-tracking
description: Set up and track OKRs (objectives and key results) and internal team or company goals in Tability. Drafts measurable objectives and key results, creates the plan, objectives and outcomes in Tability, and runs regular progress check-ins. Use this whenever someone wants to track goals, set up OKRs, run quarterly or annual planning, write key results or KRs, update goal progress, or keep a team accountable to targets, even if they never say "OKR" or "Tability". Also use for follow-ups like "update my KR", "check in on our goals" or "add a key result to Q3".
compatibility: Requires the Tability MCP connector (tools named tability_*).
---

# OKR and goal tracking in Tability

Help the user turn intentions into well-formed OKRs, store them in Tability, and keep them current with check-ins. Tability is the system of record: drafts live in chat until the user approves them, then they go into Tability.

## Before writing anything

1. Find the workspace, unless the user named one. Call `tability_list_workspaces` to see the workspaces this connection can use; pick the only one, or ask which. If that tool isn't available, ask for the workspace slug: the part after `tability.app/` in the user's Tability URL. A `403 insufficient_scope` error means this connection wasn't authorized for that workspace; see the `tability-getting-started` skill.
2. Look for existing work so you don't duplicate it: `tability_search_plans` for the period, then `tability_get_plan_content` on any match. Prefer adding to an existing plan over creating a parallel one. Skip sample plans and outcomes (`is_dummy: true`, owners marked "(sample)") unless the user asks about them.

## Drafting OKRs

Write drafts in chat first and get approval before creating anything. Good OKRs follow a few rules, and explaining them briefly helps users write better ones next time:

- **Objective**: qualitative, inspiring, one sentence, no number in it. "Make onboarding effortless for new customers."
- **Key results**: 2 to 5 per objective, each measurable with a start value and a target. "Raise activation rate from 32% to 50%." A key result is an outcome, not a task; if it reads like a to-do ("Launch the new tour"), make it an initiative instead.
- Every key result needs an owner. Ask who, rather than defaulting to the person you're talking to.
- Flag vague targets ("improve", "increase") and offer a concrete number.

Pick the Tability outcome type that matches the metric:
- `improve_metric` for move-from-X-to-Y targets (set `from`, `to`, and `score_format`, e.g. `_number_%` or `$_number_`)
- `stay_above` / `stay_below` for health thresholds
- `no_metric` for binary milestones
- `confidence` when progress can only be judged, not measured

## Creating in Tability

After the user approves the draft, create in this order: `tability_create_plan` (if needed), then `tability_create_objective`, then `tability_create_outcome` for each key result, then `tability_create_initiative` for supporting work the user mentioned. Descriptions use simple HTML, never Markdown. Report back with what was created and the links returned.

## Check-ins

For "update my goals" or weekly check-ins:
1. Use `tability_search_outcomes` with `with_pending_checkin: true` and `archived: false` to find what needs updating. Always filter by `plan_id` or owner (`membership_id`). A workspace-wide search can return more data than fits in one response.
2. Ask for the current value of each, plus anything notable.
3. Draft each check-in: current score, confidence (green on track, yellow at risk, red off track), and a short body covering result, evidence, blocker or decision, and next step. Keep the body under 1,200 visible characters and six list items, in simple HTML.
4. **Show the drafts and get a yes before posting.** `tability_create_checkin` sends notifications, Slack messages and webhooks that cannot be recalled.

## What to avoid

- Don't invent metric values or confidence levels. Ask.
- Don't create owners, plans or outcomes the user hasn't confirmed.
- Business data only; never record personal, medical, payment-card or credential data in goals or check-ins.
