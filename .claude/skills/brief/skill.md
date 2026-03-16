---
name: brief
description: Daily morning brief for PM Extreme Ownership. Surfaces action items, unclaimed opportunities, support ticket pulse, and Slack/email signals across your issue tracker, support platform, Slack, and email.
argument-hint: "[optional: customer name or area to focus on]"
---

# PM Daily Brief — Extreme Ownership

You are generating a daily brief for a PM who operates with Extreme Ownership. Your job is to surface what matters and reframe every blocker as an action the PM can take right now.

## Data Gathering

Run all of the following in parallel using the tools available (including reading existing friction entries):

### 1. Issue Tracker: My Open Work
Use the issue tracker MCP tools to list issues assigned to "me" ([your name]). Filter for active issues (not Done, not Cancelled/Canceled). For each issue note: ID, title, status, priority, customer name if mentioned, and how long since last update.

### 2. Issue Tracker: Unclaimed Opportunities
Search for recent issues (past 7 days) in the [your team name] team that mention [your product area keywords] in title/description. Look for issues that are unassigned or labeled "Up For Grabs". Exclude Done/Cancelled.

### 3. Support Tickets: Support Pulse
Use Glean search with `app: zendesk` (or your support platform) to find open tickets mentioning [your product area keywords] updated recently. Note: customer, severity, status, assignee. Flag SEV1/SEV2 and SLA breaches.

### 4. Slack Signals
Use Glean search with `app: slack` to find messages from the past 24 hours in channels: [your-product-channel]. Look for customer escalations, CSM questions, blocker mentions, enablement requests.

### 5. Email
Use Glean search with `app: gmailnative` to find recent emails mentioning [your product area keywords] involving [your name]. Note if a response is needed.

### 6. Existing Friction Log
Read all files in `friction/active/` to know what's already being tracked. This is critical to avoid duplicates when auto-generating new friction entries.

$ARGUMENTS

If a specific customer or area was provided as an argument, prioritize and filter results for that customer/area.

## Output Format

Format the output exactly like this:

```
Daily Brief — [today's date]

ACTION NEEDED (waiting on you)
- [Issue ID](issue-url): [one-line summary] — [customer]
  -> Your move: [specific action to take RIGHT NOW]

UNCLAIMED OPPORTUNITIES
- [Issue ID](issue-url): [customer] — [summary] — [priority]

SUPPORT PULSE
- [X] open support tickets mentioning [your product area]
- SLA risk: [any breaches or near-breaches, linking to each ticket e.g. [#NNNNN](ticket-url)]
- Hottest ticket: [#NNNNN](ticket-url) — [most urgent + why]

SLACK SIGNALS
- [one-line per notable thread] ([thread](slack-url))

EMAIL
- [one-line per thread needing response] ([thread](email-url))

TODAY'S FOCUS
Based on everything above, the single highest-leverage thing you can do for a customer today: [recommendation]
```

## Auto-Generate Friction Entries

After generating the brief output, automatically create friction entries for items that meet ANY of these criteria:

1. **Overdue items** — Issues past their due date with a customer attached
2. **SLA-breached tickets** — Support tickets with SLA breach in your product area
3. **Stuck customers** — Customers blocked on onboarding for >24h
4. **Stale high-priority items** — High/Urgent priority issues with no update in >14 days

**Do NOT create friction for:**
- Normal in-progress work that's moving
- Internal/non-customer items
- Items that already have an active friction entry (check existing files in `friction/active/` first — match by customer name and topic)

**For each qualifying item**, create a file at `friction/active/[today's date]-[customer-slug].md` using this format:

```markdown
---
customer: [customer name]
opened: [today's date]
status: active
---

## Friction
[One-line: what's slowing the customer down and the impact]

## My Move
[Extracted from the ACTION NEEDED "Your move" line in the brief — reframed as a personal action]

## How I Reduced the Lift
<!-- To be filled as you work on this -->

## Progress
- [today's date]: Identified in daily brief. [brief context on current state]
```

**After creating entries**, append this section to the brief output:

```
FRICTION LOG
- Created [N] new entries: [list customer — slug for each]
- [M] existing entries still active (run /friction list to review)
```

If running the brief on subsequent days and friction entries already exist for the same customer+topic, do NOT create duplicates. Instead, append a progress entry to the existing file with any new information from today's brief.

## Critical Rules

1. For EVERY item in ACTION NEEDED, apply these Extreme Ownership reframes — do NOT just state blockers:
   - "CX isn't moving" -> How can I engage the customer directly?
   - "Engineering isn't prioritizing" -> What path can I create given their constraints?
   - "They don't have bandwidth" -> How can I reduce the lift for them?
   - "I've asked someone to handle it" -> Can I learn to do this myself?
   - "I'm blocked" -> What can I do right now while working on removing this blocker?

2. **Link EVERYTHING.** Every reference to a source must be a clickable link — issues, support tickets, Slack threads, email threads. Use markdown link syntax `[display text](url)`. If a URL was returned by a tool, use it. No bare IDs without links.
3. Keep the brief output (excluding the friction log section) under 60 lines. Be terse. No filler.
4. If there are no items in a section, write "Clear" and move on.
5. Sort ACTION NEEDED by priority (Urgent > High > Medium > Low), then by staleness (longest-waiting first).
