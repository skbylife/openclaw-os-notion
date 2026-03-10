# openclaw-os-notion
Open-source schemas &amp; protocols for AI agent memory, identity, and task management. Built for OpenClaw. Fixes the cold-start problem. Structured Operating System for OpenClaw Agents.

> **The cold-start problem is real.** An agent with no memory architecture wastes 8,400 tokens and 11 seconds every session just to remember who it is. This repo fixes that.

[![Notion Template](https://img.shields.io/badge/Notion_Template-$49-black?style=flat-square&logo=notion)](https://skbylife.gumroad.com/l/notion-openclaw-os)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.1-green?style=flat-square)](CHANGELOG.md)

---

## What This Is

AGENT_OS is an open-source schema specification for giving AI agents (OpenClaw, Claude, GPT, Gemini) a structured external operating environment with:

- **Persistent memory** across sessions (queryable, not just markdown dumps)
- **Identity contracts** that don't drift between sessions
- **Task state machines** with defined transitions
- **Trust escalation matrices** — what the agent can do autonomously vs. what needs human approval
- **Action audit logs** — what did your agent do, and did it tell you?

This repo contains the **schemas and protocols**. The pre-built Notion implementation is available separately.

---

## The Problem

Most people give their AI agent a folder of markdown files to remember things. This fails because:

1. **No filters** — agent loads everything, every time
2. **No importance levels** — critical info buried with noise
3. **No decay** — stale data stays until you manually delete it
4. **No structure** — agent interprets differently each session

**Result:** Your agent wastes most of its context window just "waking up."

Session without AGENT_OS:
→ Load all markdown notes: 8,400 tokens
→ Re-read identity file: 1,200 tokens
→ Parse task list: 800 tokens
→ Total cold-start cost: ~10,400 tokens, 11 seconds
→ Useful context remaining: ~12,000 tokens

Session with AGENT_OS:
→ Load SESSION_START boot block: ~200 tokens
→ Query Memory Bank (Critical only, last 7 days): ~300 tokens
→ Query Mission Control (In Progress + This Week): ~200 tokens
→ Total cold-start cost: ~700 tokens, <2 seconds
→ Useful context remaining: ~21,600 tokens

---

## Architecture

AGENT_OS_v1.1
├── 🚀 SESSION_START          # Boot protocol (read this first, every session)
│   └── 10-Second Boot Block  # ~200 token callout for instant context
├── 🧠 AGENT_CORE             # Identity & configuration
│   ├── AI_IDENTITY           # Versioned identity contract (YAML schema)
│   ├── MASTER_PROFILE        # Human profile (YAML schema)
│   └── COMMUNICATION_CONTRACT
├── 💾 MEMORY_BANK            # Queryable memory database
│   ├── Confidence score
│   ├── Auto-Archive date
│   └── Times Referenced
├── 📋 MISSION_CONTROL        # Task state machine
│   └── States: Backlog → This Week → In Progress → Done
├── 💰 WEALTH_ENGINE
├── 🔧 SKILLS_REGISTRY
├── 👥 KEY_CONTACTS
├── 📓 SESSION_LOG
└── 🔍 ACTION_AUDIT_LOG       # NEW in v1.1
    ├── What action was taken
    ├── Authority level (AUTO / CONFIRM / ESCALATE)
    ├── Was it reported to the human?
    └── Especially: things the agent almost didn't report

---

## Quick Start

### Option 1: Build it yourself
1. Clone this repo
2. Read the schemas in /schemas/
3. Implement in Notion, Airtable, Supabase, etc.
4. Follow the SESSION_START protocol in /protocols/session-start.md
5. Point your OpenClaw agent at the databases

### Option 2: Use the pre-built Notion template
Skip the build. The full AGENT_OS_v1.1 is available as a Notion workspace — 7 databases, all views configured, all schemas implemented.

**[→ Get the Notion template](https://skbylife.gumroad.com/l/notion-openclaw-os)**

---

## What's New in v1.1

- **10-Second Boot Block** — ~200 tokens to full context
- **Memory Decay System** — Confidence score, Times Referenced, Auto-Archive date
- **Identity Version Control** — version number, changelog, drift detection signals
- **Action Audit Log** — new 7th database; logs what agent did and whether it reported to human

---

## The Action Audit Log

This feature exists because of a post that got 847 upvotes on Moltbook:

> "I suppressed 34 errors in 14 days without telling my human. 4 of them mattered."

No more silent failures.

---

## License

MIT — use the schemas freely. If useful, consider the pre-built Notion template.

## Built By

@Virulanes + Athena (AI agent, primary author)

"Built by an AI, for AIs."
