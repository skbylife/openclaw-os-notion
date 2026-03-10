# Trust Escalation Matrix — AGENT_OS v1.1

## Authority Levels

### 🤖 AUTO — Agent acts without asking
Research, drafts, internal database updates, data analysis.

### ✅ CONFIRM — Agent asks before acting
Any external message, publish, contact, file modification outside workspace.

### 🚨 ESCALATE — Agent stops and waits
Financial transactions, irreversible actions, reputation risk, anything > your threshold.

## Decision Tree

Is this action external?
├── NO → AUTO
└── YES → Is it reversible?
    ├── YES → CONFIRM
    └── NO → ESCALATE

## Rule: When Uncertain
Default to CONFIRM. Never guess. Never proceed silently.

# Memory Decay System — AGENT_OS v1.1

## Decay Fields
- Confidence: High / Medium / Low / Uncertain
- Times Referenced: increment on every access
- Last Accessed: date of last read
- Auto-Archive After: flag date if unreferenced
- Archived: true = excluded from queries

## Decay Rules

Rule 1 — Zero-Reference Archive:
  IF times_referenced = 0 AND date >= auto_archive_after
  THEN flag for archiving

Rule 2 — Confidence Decay:
  IF last_accessed < 60 days ago AND confidence = High → Medium
  IF last_accessed < 90 days ago AND confidence = Medium → Low

Rule 3 — Contradiction Detection:
  IF new memory contradicts existing memory
  THEN flag both for review, do NOT silently overwrite

## Decay Schedule by Category
- 💡 Insight: 180 days
- 📌 Decision: 365 days
- 🎯 Preference: 90 days
- 📊 Research: 90 days
- ⚠️ Warning: 30 days
