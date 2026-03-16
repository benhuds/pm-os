# Glean Agent: Daily Morning Brief

## Setup Instructions
1. Go to Glean Agents -> Create New Agent
2. Set trigger: **Schedule** (weekday mornings, your preferred time)
3. Set output: **Slack DM to yourself**
4. Copy the steps below into the agent configuration

> **Before you start**: Replace all `[placeholder]` tokens below with your specifics.

---

## Agent Name
**PM Daily Brief — Extreme Ownership**

## Agent Description
Surfaces the most important signals for your day across your issue tracker, support platform, Slack, and email. Applies the Extreme Ownership mindset: for every blocker, suggests what YOU can do right now.

---

## Step 1: My Open Work (Issue Tracker)

**Prompt:**
Use Glean Search to find issues assigned to [your name] that are currently In Progress, In Review, Waiting on information, Triage, or On Hold. For each issue, extract: title, issue ID, status, customer name (if mentioned), priority, and how long it's been in its current state. Sort by priority (Urgent > High > Medium > Low).

**Memory:** None

---

## Step 2: Unclaimed Opportunities

**Prompt:**
Use Glean Search to find issues in the [your team name] team from the past 7 days that mention [your product area keywords] in the title or description, and that either have no assignee or are labeled "Up For Grabs". Exclude issues with status Done, Cancelled, or Canceled. For each, note: title, issue ID, customer name, priority, and date created.

**Memory:** None

---

## Step 3: Support Ticket Pulse

**Prompt:**
Use Glean Search to find support tickets mentioning [your product area keywords] that were updated in the past 24 hours. For each ticket, note: customer name, severity, status, assignee, and a one-line summary. Flag any tickets that are SEV1 or SEV2, any tickets that have breached SLA (look for labels like "breached" or "5_day_ttr_breached"), and any tickets where the customer is waiting on your team.

**Memory:** Previous steps

---

## Step 4: Slack Signals

**Prompt:**
Use Glean Search to find Slack messages from the past 24 hours in these channels: [your-product-channel], [your-team-channel]. Look for: customer escalations, questions from CSMs/CX about [your product area], blockers mentioned by anyone, and new customer enablement requests. Summarize each thread in one line with the person who posted and the core ask.

**Memory:** Previous steps

---

## Step 5: Email Threads

**Prompt:**
Use Glean Search to find recent emails (past 24 hours) mentioning [your product area keywords] that involve [your name]. Note sender, subject, and whether a response is needed.

**Memory:** Previous steps

---

## Step 6: Synthesize Daily Brief

**Prompt:**
You are helping a PM who operates with an Extreme Ownership mindset. Using all the information gathered in previous steps, create a daily brief with these sections:

**Format the output exactly like this:**

```
Daily Brief — [today's date]

ACTION NEEDED (waiting on you)
For each item:
- [Issue/ticket ID]: [one-line summary] — [customer]
  -> Your move: [specific action you can take RIGHT NOW]

UNCLAIMED OPPORTUNITIES
- [Issue ID]: [customer] — [one-line summary] — [priority]

SUPPORT PULSE
- [X] open support tickets mentioning [your product area]
- SLA risk: [any breaches or near-breaches]
- Hottest ticket: [most urgent one + why]

SLACK SIGNALS
- [one-line per notable thread]

EMAIL
- [one-line per thread needing response]

TODAY'S FOCUS
Based on everything above, here is the single highest-leverage thing you can do for a customer today: [recommendation]
```

IMPORTANT RULES:
1. For every item in ACTION NEEDED, do NOT just state the blocker. Apply these reframes:
   - Instead of "CX isn't moving" -> suggest how to engage the customer directly
   - Instead of "Engineering isn't prioritizing" -> suggest a path given their constraints
   - Instead of "They don't have bandwidth" -> suggest how to reduce the lift
   - Instead of "Waiting on someone" -> suggest what to do RIGHT NOW while waiting
2. Keep the entire brief under 50 lines. Be terse. No filler.
3. If there are no items in a section, write "Clear" and move on.

**Memory:** All previous steps
