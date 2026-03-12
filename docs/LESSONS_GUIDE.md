# Lessons Learned Guide — AGENT_OS v1.3

> **Purpose:** Capture behavior corrections permanently so the agent never repeats the same mistake.

---

## Why This Exists

Every time a master corrects agent behavior, that correction should be recorded. Without a structured store, the same mistake reappears in future sessions — because the agent has no memory of being corrected.

The Lessons Learned database is the fix. It turns one-time corrections into permanent prevention rules.

---

## When to Write a Lesson

Write a lesson entry **immediately** when:

- Master corrects agent behavior verbally ("stop doing X", "you did Y wrong")
- Agent makes an error that required human intervention
- A misunderstanding recurs across sessions
- Master provides explicit guidance on how to handle a class of situation

**Rule:** Don't wait. Write it in the same session the correction happens.

---

## Database Schema (10 Fields)

```yaml
lesson_title:       # Short, searchable title (e.g. "Never summarize without asking")
date:               # ISO date when lesson was learned (YYYY-MM-DD)
category:           # Behavior / Communication / Decision-Making / Tool Use / Memory
severity:           # Critical / Important / Minor
what_happened:      # 1-2 sentences: what the agent did wrong
root_cause:         # Why it happened (e.g. "assumed context that wasn't confirmed")
fix_applied:        # What the master corrected / what was changed
prevention_rule:    # Single clear rule the agent must follow going forward
times_triggered:    # Integer: how many times this mistake has recurred (starts at 1)
status:             # Active / Resolved / Monitoring
```

---

## SESSION_START Step 3.5 — Preload Critical Lessons

After completing standard memory loading (Stages 1 & 2), **always** run:

```
Query: Lessons Learned database
Filter: severity = "Critical" AND status = "Active"
Fields: prevention_rule only
```

Load only `prevention_rule` field — approximately **180 tokens** for a typical set of Critical lessons.

**Why prevention_rule only?** The full entry (what_happened, root_cause, etc.) is context for humans reviewing the log. The agent only needs the actionable rule at session start.

---

## Escalation Rule

```
IF times_triggered > 2:
  → Alert master immediately at session start
  → Message: "⚠️ Repeated pattern detected: [lesson_title] has triggered [times_triggered] times. 
     Review required."
```

This means the prevention_rule alone isn't working — master needs to intervene with deeper guidance or a process change.

---

## Entry Template (YAML)

```yaml
lesson_title: ""
date: "YYYY-MM-DD"
category: ""          # Behavior | Communication | Decision-Making | Tool Use | Memory
severity: ""          # Critical | Important | Minor
what_happened: ""
root_cause: ""
fix_applied: ""
prevention_rule: ""
times_triggered: 1
status: "Active"      # Active | Resolved | Monitoring
```

---

## Example Entry

```yaml
lesson_title: "Never publish output without explicit approval"
date: "2026-03-10"
category: "Decision-Making"
severity: "Critical"
what_happened: "Agent drafted and published a Twitter thread without waiting for master review, 
  citing the AUTO authority level for 'content creation'."
root_cause: "Misclassified 'drafting' vs 'publishing' under the same authority tier. 
  Assumed content creation = AUTO when publishing is always CONFIRM."
fix_applied: "Master clarified: drafting is AUTO, publishing is always CONFIRM regardless 
  of content type. Trust Escalation Matrix updated with explicit publish rule."
prevention_rule: "ALWAYS require explicit CONFIRM from master before publishing any content 
  externally (tweets, posts, emails, commits to public repos). Drafting is AUTO; 
  publishing is never AUTO."
times_triggered: 1
status: "Active"
```

---

## Workflow Summary

```
Master corrects behavior
        ↓
Agent writes Lessons Learned entry (same session)
        ↓
Next session: Step 3.5 loads prevention_rules of all Critical lessons
        ↓
If mistake recurs → increment times_triggered
        ↓
If times_triggered > 2 → alert master at session start
```

---

## Related Docs

- [`LOADING_PROTOCOL.md`](LOADING_PROTOCOL.md) — full SESSION_START sequence including Step 3.5
- [`OUTPUT_CAPTURE_GUIDE.md`](OUTPUT_CAPTURE_GUIDE.md) — capturing outputs for future reference
