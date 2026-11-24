---
name: tability-getting-started
description: Get started with Tability in Claude. Connects and signs in to Tability, creates an account for new users, picks a workspace, sets up a first plan of goals, and shows what the Tability plugin can do. Use this whenever someone wants to set up, connect, try or sign in to Tability, asks what the Tability plugin or connector can do, says they don't have a Tability account yet, or when a Tability tool fails because it is not connected, not authenticated or unavailable.
compatibility: Requires the Tability MCP connector (tools named tability_*).
---

# Getting started with Tability

Get the user from "just installed" to a working Tability setup with their first goals in it, in as few steps as possible. People arrive here at different points: some already use Tability, some have never heard of it, and some hit a connection error in the middle of another task. Work out which, and skip whatever they don't need.

## 1. Check the connection

Call `tability_whoami`.

- **It works**: greet them by name, then go to step 3.
- **The tool doesn't exist**: the connector isn't enabled. In Claude Code, run `/plugin` and check that the `tability` plugin is enabled, then `/mcp` to see the server. In Claude apps, turn on the Tability connector from the connectors settings. Then try again.
- **It fails with an authentication error** (401, missing token, needs auth): they need to sign in. In Claude Code, run `/mcp`, select `tability` and choose to authenticate. A browser window opens for the Tability sign-in. In Claude apps, connect Tability from the connectors settings. Then call `tability_whoami` again.

Never ask for a password, API key or token in chat. Sign-in always happens in the browser through OAuth.

## 2. New to Tability

If they don't have an account, say in two or three sentences what Tability is: a place where teams set OKRs and goals, track KPIs, and keep a weekly check-in rhythm, so progress and risks are visible without status meetings.

Then give them the sign-up link, which starts a free trial:

https://tability.app/signup?utm_source=claude&utm_medium=plugin&utm_campaign=getting-started

Once they've created their account and workspace, go back to step 1 to sign in.

Don't make them wait for the account to get value. If they came with goals in mind, offer to draft them in chat while they sign up, so the drafts are ready to save once they're connected.

## 3. Pick the workspace

Call `tability_list_workspaces`. It returns the workspaces this connection can use, with slug, name and the user's role. If there's one, use it; if there are several, ask which. If the list is empty, or the workspace they want is missing, the workspace wasn't selected at sign-in: follow the reconnect steps below.

If `tability_list_workspaces` isn't available (older servers), ask for the workspace slug: the part after `tability.app/` in their Tability URL (for example `acme` in `tability.app/acme`), and check it with `tability_get_workspace`.

Remember the workspace for the rest of the conversation.

If that call fails with **`403 insufficient_scope`**, this connection wasn't authorized for that workspace. During sign-in, Tability asks which workspaces to share, and this one wasn't ticked (or the slug is wrong). To fix it:

- In Claude Code, run `/mcp`, select `tability`, and reconnect (or clear authentication and sign in again). On the Tability consent screen, tick the workspace.
- In Claude apps, disconnect and reconnect Tability from the connectors settings, tick the workspace, then start a new conversation. An existing conversation can keep using the old connection.
- If the workspace isn't offered on the consent screen, a workspace admin needs to enable API access for the user's membership.

## 4. First plan

Call `tability_search_plans` with `archived: "false"` to see what exists. New workspaces often start with sample plans (`is_dummy: true`, titles ending "(sample plan)"). Treat a workspace with only sample plans as empty, and mention that the samples are there to explore.

- **Real plans already exist**: summarise them in a line each (name, period, number of key results) and ask what they want to do next. Skip to step 5.
- **The workspace is empty, or has only sample plans**: offer a quick start. Ask three things, one message at a time if needed:
  1. Which team or scope (company, a team, or just themselves)?
  2. Which period (this quarter, next quarter, this year)?
  3. What are the one to three things that matter most in that period?

  Draft a plan from the answers: one objective per priority, and two or three measurable key results for each, with a start value and a target. Follow the drafting rules in the `tability-okr-tracking` skill. Show the draft and get approval. Then create it with `tability_create_plan`, `tability_create_objective` and `tability_create_outcome`, using simple HTML in descriptions. Share the links that come back.

If key results belong to teammates, tell the user to invite them to the workspace from Tability, so they can own their goals and post check-ins. Invitations are done in the Tability app, not through Claude.

## 5. Show what's possible

End with a short menu of what they can ask next, using their own plan names where possible:

- "Check in on my key results": weekly progress updates (OKR tracking)
- "Write this week's status report for leadership": a report from recorded check-ins
- "Turn this goal into a SMART goal and track it": SMART goals
- "Here are this month's numbers, update our KPIs": KPI tracking and reports
- "Cascade our company goals to each team": strategy alignment and dependencies

Mention once that check-ins notify teammates, so Claude always shows a draft before posting one.

## Avoid

- Don't create plans, objectives or outcomes before the user approves a draft.
- Don't repeat this setup for someone who is already connected and asking for something else. Answer the actual request.
- Don't describe pricing, plans or limits beyond "free trial". Point to the sign-up page for details.
