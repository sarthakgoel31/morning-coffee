---
name: morning-coffee
description: "Daily morning planner — checks calendar, email, Slack, and projects to build an action plan for the day. Optionally pulls 6E trading levels + game plan. Use when someone says 'morning coffee', 'plan my day', 'what's on today', or 'run morning coffee'."
user_invocable: true
---

# Morning Coffee

Plan Sarthak's day every morning. Pull live data from Calendar, Gmail, and Slack, cross-reference with active projects, then produce a detailed but organized morning briefing.

## Trading Integration

- If the user's prompt mentions "levels", "trade", "trading", "6E", or "morning prep" → include trading levels + game plan automatically.
- If the user just says "morning coffee" / "plan my day" with no trading mention → show the day plan FIRST, then ask: **"Want me to pull today's 6E levels and trade plan too?"**
- When trading IS requested: start the Windows PC data fetch (`ping 192.168.1.26` → `curl http://192.168.1.26:8080/6EM6.CME.scid`) in parallel with Calendar/Gmail/Slack. By the time the day plan is shown, trade data is already downloading.

## Data Sources

1. **Google Calendar** — today's events + rest of week
2. **Gmail** — unread/starred/important threads from last 24h
3. **Slack** — unread DMs and mentions from last 24h
4. **Active projects** — from CLAUDE.md and memory files at `~/.claude/projects/-Users-sarthak-Claude/memory/`
5. **Trading data** (optional) — Sierra .scid from Windows PC, then compute levels via `personal/trading-monitor/` engine

## Workflow

### Step 1: Gather data (ALL in parallel)

Launch everything at once:

- **Calendar**: `gcal_list_events` for today + next 4 days
- **Gmail**: `gmail_search_messages` with `is:unread OR is:starred newer_than:1d`, top 10
- **Slack**: `slack_search_public_and_private` for messages to Sarthak in last 24h
- **Trading** (if requested): Start `ping` + `curl` to fetch .scid from Windows PC in background

### Step 2: Build the briefing

Cross-reference all sources. Flag items that need action TODAY:
- Calendar events that need prep
- Gmail threads awaiting reply
- Slack messages needing response
- Project deadlines or blockers from memory

Day-of-week awareness:
- Monday/Tuesday: "Easing in — prioritize planning"
- Wednesday/Thursday: "Mid-week — being aggressive"
- Friday: "Wrapping up — close open loops"

### Step 3: Present output

Use this EXACT format:

```
## Morning Coffee -- [Full Date] ([Day of Week])

### Today's Calendar
- [Time] -- [Event]
(If clear: "No meetings today. Deep work day.")

### This Week Ahead
| Day | What |
|-----|------|
| Today | [summary] |
| Tomorrow | [meeting or "Clear"] |
| [next days...] | [...] |

### Action Items ([Day-awareness phrase])

Show ALL actionable items (not just top 5). Each item gets:
- **Bold name** -- One-line summary of what it is
  - 2-3 lines of context: who's involved, what's the history, why it matters
  - **Action:** Exactly what Sarthak should do, in concrete terms

Number them. Group by urgency (most urgent first).

After all actionable items, collapse noise:
> +[N] skipped: [list of newsletters, promos, digests, birthday emails]

---

### Day Plan

| Time | Block |
|------|-------|
| [slot] | [activity -- include specific sub-tasks, not just generic labels] |

The day plan should:
- Map every action item to a specific time slot
- Include specific sub-tasks in each block (e.g., "Reply Priyanka on QR tracking, approve Finnoto expense")
- Have a "Quick hits" block for 2-minute tasks
- Include gym, lunch, wrap-up blocks
- End with a wrap-up slot for clearing remaining Slack DMs

> Looks good? Any changes?
```

If trading was requested, append AFTER the day plan:

```
---

### 6E Trading Levels

**Previous Day:** H [x] | L [x] | C [x] | Range [x] pips
**Current:** [price] | **VWAP:** [x] ([distance] from price)

| Level | Price | Distance | Cluster |
|-------|-------|----------|---------|
Show ALL levels — Important clusters, solo levels, everything sorted high to low.
Bold the key zone row.

**Indicators:** ATR [x] pips | RSI [x] | Delta [x] | CumDelta [x]

**Bias:** [2-3 sentences: VWAP position, delta read, price action context, what the market is doing]

**Key zone:** [Which level matters most and why. What happens if it holds vs breaks.]

**Upside targets:** [First and second resistance levels with distance]

> **Game plan:** [3-4 sentences: What to look for, entry conditions, what disqualifies a trade, session window, max trades]
```

## Rules

1. **Time zone**: IST always
2. **Schedule**: Wakes ~8 AM, works 9-8 PM, gym lunch or evening
3. **Trading window**: Block 8:00-12:00 IST on weekdays if trading is included
4. **Detail level for action items**: Every actionable item gets context + a specific "Action:" line. Don't be terse here — Sarthak wants to understand what each item is about without having to go look it up. Include names of people, what they said, and what the right next step is.
5. **Collapse noise, not action**: Newsletters, promos, birthday emails, digests → collapse into the "+N skipped" line. Everything that needs a reply or decision → show in full.
6. **Tone**: Warm but direct. Organized so it feels manageable, not overwhelming. Use structure (headers, tables, sub-bullets) to create visual breathing room.
7. **Bold item names**, `--` separators, table for schedule.
8. **Trading section is detailed**: Show all levels (Important clusters + solo). Include prev day stats, full indicator line, multi-sentence bias, key zone analysis with upside/downside scenarios, and a full game plan paragraph.
9. **Ask, don't assume**: If trading wasn't mentioned, ask. Don't dump levels by default.
10. **Graceful degradation**: If Gmail/Slack/Windows PC unavailable, skip with a note and continue.
11. **Week ahead table**: Always show the next 4-5 days so Sarthak knows what's coming.
12. **Day plan specificity**: Every time block should say exactly what to do, not just "deep work." Name the project, the task, the person to sync with.
