# Setup Guide

This OS is designed to be forked and configured for your team. Here's how to get it running.

## Prerequisites

- [Claude Code](https://claude.ai/claude-code) CLI installed
- MCP servers configured for your tools:
  - **Issue tracker** (Linear, Jira, etc.)
  - **Knowledge search** (Glean, or equivalent for searching across Slack, support tickets, email, call recordings)
- Access to Glean (for the daily brief agent — optional, the `/brief` skill works without it)

## Step 1: Fork and Clone

```bash
git clone <your-fork> ~/pm-os
cd ~/pm-os
```

## Step 2: Configure Skill Files

Each skill file contains `[placeholder]` tokens that you need to replace with your specifics.

### `/brief` skill (`.claude/skills/brief/skill.md`)

Replace these placeholders:
| Placeholder | Example | Description |
|---|---|---|
| `[your name]` | `Jane Smith` | Your name as it appears in your issue tracker |
| `[your product area keywords]` | `"data pipeline", "ingestion", "ETL"` | Keywords for searching support tickets and Slack |
| `[your-product-channel]` | `#team-data-platform` | Slack channels to monitor |
| `[your team name]` | `Customer Requests` | Issue tracker team/project to search for unclaimed work |

### `/weekly` skill (`.claude/skills/weekly/skill.md`)

Same placeholders as `/brief`, plus:
| Placeholder | Example | Description |
|---|---|---|
| `[your manager]` | `Sarah Chen` | Who the 1:1 doc is shared with |

### `/brief` — Team Board (new in v2)

The brief now scans your team's customer issues board for full visibility, even on issues not assigned to you. Configure:

| Placeholder | Example | Description |
|---|---|---|
| `[your team board team]` | `Platform` | The team/project in your issue tracker that holds customer issues |
| `[your team board label]` | `Customer Issue` | The label used to tag customer-facing issues |

### `/brief` — Slack Mentions (new in v2)

The brief searches for threads where you were tagged and classifies them as needing your input or just monitoring. Configure:

| Placeholder | Example | Description |
|---|---|---|
| `[your name]` | `Jane Smith` | Your name as it appears in Slack mentions |

### `/actions` skill (`.claude/skills/actions/skill.md`)

No placeholders needed — this skill is fully generic. It reads/writes `actions/week-YYYY-MM-DD.md` files and cross-references your friction log.

### Glean Agent (`glean-agent-daily-brief.md`)

Same keyword and channel replacements. Follow the setup instructions in the file to create the Glean Agent.

## Step 3: Initialize Friction Log

The directories are already set up:
```
friction/
  active/      # Your working friction entries
  dissolved/   # Resolved friction (auto-moved by /friction dissolve)
```

Run `/brief` to auto-seed your first friction entries, or add one manually:
```
/friction add [customer] can't do [thing] — [what you're doing about it]
```

## Step 4: Test It

```bash
cd ~/pm-os

# Run your first brief
/brief

# Check the friction log
/friction list

# Log an action
/actions prepped onboarding doc for Acme

# See your scorecard
/actions list

# On Friday, run the weekly reflection
/weekly
```

## Step 5: Make It a Habit

- **Morning**: Check the Glean Agent DM, or run `/brief`
- **Throughout the day**: `/actions log [what you did]` to track your moves
- **When a customer is stuck**: `/friction add [details]`
- **When something resolves**: `/friction dissolve [customer]`
- **End of week**: `/actions carryover` to clean up stale items
- **Friday**: `/weekly` to pre-fill your reflection, then paste into your 1:1 doc

## Adapting for Different Tools

The skills are written for Linear + Glean + Zendesk, but the pattern works with any tools that have MCP servers:

- **Jira instead of Linear**: Update the issue tracker queries in the skill files
- **No Glean**: Use individual MCP servers for Slack, Gmail, etc. — adjust the search calls accordingly
- **No Gong**: Remove the call recording search from `/weekly`, or replace with your recording tool
- **Different support tool**: Replace Zendesk references with your support platform

The friction log and mindset are tool-agnostic — they work regardless of your stack.
