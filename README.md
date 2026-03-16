# PM OS — Extreme Ownership Operating System

A CLI-native operating system for Product Managers who own outcomes, not tasks. Built on [Claude Code](https://claude.ai/claude-code) and the Extreme Ownership mindset.

## The Problem

Without a system, weeks go by and you can't tell if you're moving forward or just staying busy. You check Slack, scan your issue tracker, skim emails — but it's reactive. The Extreme Ownership mindset asks you to operate differently:

- Own outcomes, not tasks
- Dissolve friction, don't document it
- Make it easy for others to help
- Close the gap between what you've built and what customers need

This OS makes that concrete and habitual.

## How It Works

The OS runs on a **Sense > Act > Reflect** loop:

```
+-------------------------------------------------+
|  SENSE (automated)                              |
|  /brief surfaces what needs attention across    |
|  your issue tracker, support tickets, Slack     |
|  mentions, team board, email, and calls         |
+-------------------------------------------------+
|  ACT (you + tracked)                            |
|  Friction log tracks customer blockers          |
|  Action tracker logs what you did each day      |
|  /actions log captures your moves as you go     |
+-------------------------------------------------+
|  REFLECT (automated + you)                      |
|  /weekly pulls from actions + friction to       |
|  pre-fill your 1:1 doc with real evidence       |
+-------------------------------------------------+
```

## Components

### 1. Daily Morning Brief (Glean Agent)
A scheduled Glean Agent that DMs you on Slack each morning with:
- **Action needed**: Items waiting on you, with Extreme Ownership reframes (not "I'm blocked" but "here's what I can do right now")
- **Unclaimed opportunities**: New customer requests you could pick up
- **Support pulse**: Support ticket trends, SLA risks
- **Slack/email signals**: What's being discussed about your area

See [`glean-agent-daily-brief.md`](./glean-agent-daily-brief.md) for the full agent configuration.

### 2. `/brief` — Ad-hoc Brief (Claude Code Skill)
Same intelligence as the morning DM, but runnable on-demand from your terminal:
```
/brief
```
Use this when you're context-switching, before a meeting, or when you feel like you might be missing something.

The brief also **auto-generates friction entries** from what it finds — overdue items, SLA breaches, stuck customers — so your friction log stays populated without manual effort.

### 3. `/weekly` — Friday Reflection (Claude Code Skill)
Pre-fills the three questions from the Extreme Ownership mindset using your actual data:
```
/weekly
```

Answers:
1. **What customer value did we create this week?** — Numbers, not activities. Pulls from issue tracker completions, adoption metrics, and customer outcomes.
2. **What friction did we remove?** — For the customer, not internally. Reads from your friction log.
3. **What's one customer story we can tell?** — Searches call recordings/Slack for the best anecdote backed by data.

Also includes support ticket trends (new vs resolved, top categories, SLA health) and an adoption gap analysis.

Output is formatted to paste directly into your 1:1 doc and team weekly updates.

### 4. `/actions` — Action Tracker (Claude Code Skill)
Tracks what you did each day against brief signals. One weekly file, no sprawl.
```
/actions prepped package for Sarah     # Log what you did (auto-augmented)
/actions list                          # Scorecard with linked sources
/actions carryover                     # Review stale items from earlier in the week
```

The brief auto-generates action items each morning. You log completions throughout the day. On Friday, `/weekly` pulls from the same file — no double-entry.

The action tracker cross-references the friction log: friction items appear on the scorecard so nothing falls through the cracks between the two systems.

### 5. Friction Log
A living system for tracking and dissolving blockers. Not a backlog — every entry has a **"My Move"** field that captures what YOU are doing, not who you're waiting on.

```
friction/
  active/       <-- Current friction you're working to dissolve
  dissolved/    <-- Friction you've dissolved (with how)
```

Each entry tracks:
- **Friction**: What's slowing the customer down
- **My Move**: What you're doing right now
- **How I Reduced the Lift**: What you did to make it easier for eng/CX/others
- **Progress**: Timeline of updates

Manage it with:
```
/friction            # List active friction
/friction add        # Add new entry
/friction update     # Log progress on an entry
/friction dissolve   # Move to dissolved with summary
```

## Directory Structure

```
pm-os/
+-- README.md                          # This file
+-- SETUP.md                           # How to configure for your team
+-- glean-agent-daily-brief.md         # Copy-paste config for Glean Agent
+-- .claude/
|   +-- skills/
|       +-- brief/skill.md             # /brief -- ad-hoc daily brief
|       +-- weekly/skill.md            # /weekly -- Friday reflection
|       +-- friction/skill.md          # /friction -- manage friction log
|       +-- actions/skill.md           # /actions -- weekly action tracker
+-- actions/                           # Weekly action tracker files (one per week)
+-- friction/
|   +-- active/                        # Current friction entries
|   +-- dissolved/                     # Dissolved friction (with how + when)
+-- templates/
|   +-- friction-entry.md              # Template for new friction entries
|   +-- weekly-reflection.md           # Template for weekly reflection
+-- examples/
    +-- friction-entry-example.md      # Example friction entry (synthetic)
```

## The Anti-Patterns

The Extreme Ownership mindset calls out conversations we need to stop having. This OS is designed to make the right conversation the default:

| Instead of saying...                        | The OS helps you...                                                    |
|---------------------------------------------|------------------------------------------------------------------------|
| "CX isn't moving."                          | `/brief` surfaces the customer thread so you can engage directly       |
| "Engineering isn't prioritizing this."      | Friction log asks "what path can I create given their constraints?"     |
| "They don't have bandwidth."               | Friction log tracks "how I reduced the lift for them"                  |
| "I've asked someone to handle onboarding." | `/brief` flags stale handoffs so you can learn to do it yourself       |
| "I'm blocked."                             | Daily brief reframes every blocker with "what can you do right now?"   |

## Getting Started

See [SETUP.md](./SETUP.md) for full configuration instructions.

### Quick Start

1. **Fork this repo** and make it your own
2. **Search for `[placeholder]` tokens** in skill files and replace with your team's specifics
3. **Configure your MCP servers** for Claude Code (Linear, Glean, etc.)
4. **Run `/brief`** to test your first daily brief

## The Weekly Scorecard

Every Friday, you should be able to answer these three questions with data:

| Question | Where the answer comes from |
|---|---|
| What customer value did we create this week? | Issue tracker completions + adoption metrics + customer outcomes |
| What friction did we remove? | `friction/dissolved/` entries from this week |
| What's one customer story we can tell? | Call recordings + Slack threads + support ticket resolutions |

If you can't answer all three, that's the signal to change something next week.

---

*Built on the Extreme Ownership mindset.*
