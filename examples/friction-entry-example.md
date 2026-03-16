# Example Friction Entry

This is a synthetic example showing what a friction entry looks like in practice. Your real entries live in `friction/active/`.

---

```markdown
---
customer: Acme Corp
opened: 2026-03-10
status: active
---

## Friction
Acme can't complete onboarding — their Snowflake setup fails with a permissions error. Blocking first value. SEV2 open for 5 days.

## My Move
Reproduced the error locally and wrote a step-by-step fix. Posting directly to the support ticket instead of waiting for eng to triage.

## How I Reduced the Lift
Wrote a repro script and attached it to the Linear issue so eng can validate the fix without debugging from scratch.

## Progress
- 2026-03-12: Posted workaround to support ticket. Customer confirmed it works. Filing a docs update so this doesn't recur.
- 2026-03-11: Reproduced the permissions error. Root cause: docs show the wrong IAM role. Wrote fix script.
- 2026-03-10: Identified in daily brief. ZD#12345 open 5 days, SLA breached. Customer waiting on us.
```

---

After dissolving, the entry would be moved to `friction/dissolved/` with a final progress note:

```markdown
- 2026-03-13: DISSOLVED. Docs updated with correct IAM role. Support ticket closed. Filed LIN-456 to add validation check during setup so this class of error is caught automatically.
```
