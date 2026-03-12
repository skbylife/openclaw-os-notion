# Output Capture Guide — AGENT_OS v1.3

> **Problem:** 89% of AI outputs are generated and never referenced again. Hours of synthesis vanish at session end.

---

## Why Output Capture Matters

Every session, the agent produces:
- Research summaries
- Drafted documents
- Strategic plans
- Code snippets
- Analysis outputs

Without a capture system, these are gone at session end. The next session starts from scratch — rebuilding what already existed. This is the **output amnesia problem**.

---

## The Decision Rule

Before ending any session, ask:

> **"Would master want this in 2 weeks? Would rebuilding it take more than 5 minutes?"**

If **yes to either** → capture it.

If no → discard freely.

---

## OUTPUT_CAPTURE Block Format

When capturing an output, log it to the Session Log using this structure:

```
OUTPUT_CAPTURE
  retrieval_key: [keyword1, keyword2, keyword3]   # 3-5 keywords for future search
  output_type: [Research | Draft | Plan | Code | Analysis | Other]
  summary: [1-2 sentence description of what this output contains]
  location: [inline | child_page: <page_title>]
  created: YYYY-MM-DD
  content: |
    [Full output here, OR reference to child page if >500 words]
```

### retrieval_key Design

Choose keywords a future agent would actually search for:
- Topic-based: `twitter-strategy`, `gumroad-pricing`, `notion-schema`
- Action-based: `draft`, `analysis`, `comparison`
- Project-based: `agent-os`, `wealth-engine`, `claw-academy`

**Good:** `["agent-os", "session-log", "schema-v1.3"]`  
**Bad:** `["output", "content", "result"]`

---

## Long Output Rule

If output exceeds **500 words**:

1. Create a **child Notion page** inside the current Session Log entry
2. Title it: `[Date] — [retrieval_key keywords]`
3. Store full content in the child page
4. In the Session Log entry, record:

```
OUTPUT_CAPTURE
  retrieval_key: [keywords]
  output_type: Research
  summary: "Full competitive analysis of AI agent tools (2,400 words)"
  location: child_page: "2026-03-12 — agent-tools competitive-analysis"
  created: 2026-03-12
```

This keeps Session Log entries scannable while preserving full outputs.

---

## Anti-Amnesia Rule

**Before ever saying "I don't have that" or "I don't recall generating that":**

1. Query Session Log with relevant keywords
2. Check `Outputs Captured` field for matching retrieval keys
3. If found → retrieve from child page or inline content
4. Only after searching: acknowledge if genuinely not found

```
Search sequence:
  1. Query Session Log → filter by date range (last 30 days)
  2. Search retrieval_key field for topic keywords
  3. Check Output Types filter for relevant category
  4. Open matching entries → retrieve output
```

**Rule:** "I don't have that" is only valid after a keyword search returns no results.

---

## 3 New Session Log Fields (v1.3)

These fields were added to the Session Log database schema:

| Field | Type | Purpose |
|-------|------|---------|
| **Outputs Captured** | Rich Text | Comma-separated list of retrieval_keys from this session |
| **Output Count** | Number | How many outputs were captured this session |
| **Output Types** | Multi-select | Research / Draft / Plan / Code / Analysis / Other |

### How to Update at Session End

```
At session close:
  1. Count OUTPUT_CAPTURE blocks logged this session → set Output Count
  2. List all retrieval_keys → paste into Outputs Captured (comma-separated)
  3. Select all output types used → set Output Types
```

---

## Workflow Summary

```
Agent produces valuable output
        ↓
Apply decision rule: keep? (2-week test / 5-min rebuild test)
        ↓ Yes
Write OUTPUT_CAPTURE block with retrieval_key
        ↓
If >500 words → create child page, store reference
        ↓
At session end → update Outputs Captured, Output Count, Output Types
        ↓
Future session → search Session Log before claiming no memory
```

---

## Example: Capturing a Research Output

```
OUTPUT_CAPTURE
  retrieval_key: [gumroad, pricing, competitor-analysis]
  output_type: Research
  summary: "Comparison of 6 Notion template sellers on Gumroad — pricing, positioning, 
    bundle strategies. Used to inform AGENT_OS $49 price point decision."
  location: child_page: "2026-03-12 — gumroad pricing competitor-analysis"
  created: 2026-03-12
```

---

## Related Docs

- [`LESSONS_GUIDE.md`](LESSONS_GUIDE.md) — capturing behavior corrections
- [`LOADING_PROTOCOL.md`](LOADING_PROTOCOL.md) — SESSION_START sequence
