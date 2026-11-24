---
name: tability-kpi-reporting
description: Track and report on KPIs (key performance indicators) and business metrics in Tability. Sets up KPIs with targets, records new values, groups them into KPI views or scorecards, and writes period-over-period KPI reports that explain what moved, what missed target and why it matters. Use this whenever someone wants KPI tracking, a KPI report, a metrics review, a scorecard, a business health check, monthly or weekly metrics, or wants to log numbers like revenue, churn, NPS or pipeline against targets, even if they don't mention Tability.
compatibility: Requires the Tability MCP connector (tools named tability_*kpi*).
---

# KPI tracking and reporting in Tability

KPIs are ongoing health metrics, unlike OKRs, which are time-boxed pushes for change. Use this skill for the always-on numbers a business watches. If the user really wants to change a metric this quarter, suggest an OKR instead and say why.

## Workspace

Every call needs a workspace. If it's unclear: call `tability_list_workspaces` to see the workspaces this connection can use; pick the only one, or ask which. If that tool isn't available, ask for the workspace slug: the part after `tability.app/` in the user's Tability URL. A `403 insufficient_scope` error means this connection wasn't authorized for that workspace (see `tability-getting-started`).

## Set up KPIs

1. Check for existing ones first: `tability_search_kpis` and `tability_search_kpi_views`. Reuse rather than duplicate. If several KPIs share a name, show them and ask which one is meant.
2. For each new KPI, confirm the name, unit, owner, update frequency and target, then `tability_create_kpi`.
3. Set targets with `tability_upsert_kpi_target` for the dates the user gives.
4. Group related KPIs into a view (a scorecard per team or area) with `tability_create_kpi_view` and `tability_add_kpi_to_view`.

## Record values

For "here are this month's numbers": map each number to a KPI, show the mapping, and after the user confirms, call `tability_upsert_kpi_value` with the exact date. Upsert replaces any value already on that date, so mention it when one exists.

## Write a KPI report

1. Pull data: `tability_get_kpi_view_data` for a view, or `tability_list_kpi_values` and `tability_list_kpi_targets` per KPI over the period. Skip views and KPIs that belong to sample plans (names ending "(sample plan)") unless the user asks for them.
2. For each KPI compute current value, change vs. previous period, and gap to target. Do the arithmetic carefully and show the inputs.
   - **Targets are often stepped.** A view's `targets` array can hold the starting value for every period, then jump to the real goal in a later period. Comparing to the current period's target would then make every KPI look on target. Always also report the gap to the next target that differs from the baseline (the period-end goal), and the share of the way there: (latest − start) ÷ (goal − start).
   - Respect `target_direction`: for `down` KPIs, lower is better.
   - Use the date of the latest value, not today, and say how old the data is.
3. Structure:
   - **Summary**: overall health in two sentences.
   - **Missed or slipping**: KPIs below target or trending the wrong way, with numbers.
   - **Improving**: notable gains.
   - **No data**: KPIs not updated this period.
4. Show a chart when the environment supports one and a trend is easier to see than to read.

Don't explain causes the data doesn't show. If the user wants commentary on why a number moved, ask them or say it's a hypothesis.

## Avoid

- Deleting KPIs, values or views (`tability_delete_*`) unless the user explicitly asks; confirm first, since deleting a KPI removes its history.
- Recording sensitive data; aggregate business metrics only.
