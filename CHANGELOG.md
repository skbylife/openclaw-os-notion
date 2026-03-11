# CHANGELOG — AGENT_OS

## v1.2 — 2026-03-11

### Added
- **Two-Stage Memory Loading**: Stage 1 scans AI Context (1-liner) field only (~2 tokens/entry). Stage 2 loads Full Detail only for relevant entries. Reduces Memory Bank loading cost by 60-70%.
- **Today's Context Block**: Pre-computed daily briefing callout (~100 tokens). Fill once at session start, read instead of re-running all queries.
- **Optimized Query Patterns**: SESSION_START now includes field-specific queries with explicit token cost benchmarks.

### Changed
- SESSION_START protocol updated with 7-step sequence (was 5-step)
- Cold-start benchmark updated: v1.2 achieves ~350 tokens (vs ~700 in v1.1, vs ~10,400 without AGENT_OS)

---

## v1.1 — 2026-03-09

### Added
- **10-Second Boot Block**: Single callout at top of SESSION_START. ~200 tokens to full context.
- **Memory Decay System**: Every Memory Bank entry now has Confidence, Times Referenced, Last Accessed, Auto-Archive After, and Archived fields.
- **Identity Version Control**: AI_IDENTITY now has version number, last_updated date, changelog, and drift detection signals.
- **Action Audit Log (new database)**: 7th database. Logs what the agent did, at what authority level, and whether it was reported to the human.

### Changed
- SESSION_START protocol updated with explicit 5-step sequence
- Trust Escalation Matrix moved to AI_IDENTITY for better agent access

### Fixed
- Memory Bank entries with no importance level now default to Important

---

## v1.0 — 2026-03-01

### Initial Release
- 6 core databases: Memory Bank, Mission Control, Wealth Engine, Skills Registry, Key Contacts, Session Log
- AI Identity + Master Profile typed YAML schemas
- Communication Contract with decision authority matrix
- SESSION_START protocol (basic version)
- Trust Escalation Matrix (AUTO / CONFIRM / HUMAN_ONLY)
