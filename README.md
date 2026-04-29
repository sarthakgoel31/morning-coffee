# Morning Coffee

**Your entire day, planned in one command.**

## What It Does

Morning Coffee pulls live data from Google Calendar, Gmail, Slack, and your active projects, then assembles a detailed morning briefing with concrete action items and a time-blocked day plan. It optionally integrates 6E futures trading levels and a game plan. Instead of checking four apps and piecing together your day manually, you get everything organized in one structured output -- who needs a reply, what meetings need prep, and exactly what to work on in each time block.

## How It Works

1. You say "morning coffee" or "plan my day"
2. Four data sources are queried in parallel:
   - **Google Calendar** -- today's events plus the next 4 days
   - **Gmail** -- unread, starred, and important threads from the last 24 hours
   - **Slack** -- unread DMs and mentions from the last 24 hours
   - **Project memory** -- active project statuses and blockers from workspace memory files
3. All sources are cross-referenced. Items needing action TODAY are flagged with full context: who is involved, what they said, and exactly what to do
4. A time-blocked day plan maps every action item to a specific slot, including quick-hit batches, deep work blocks, and a wrap-up slot
5. Day-of-week awareness adjusts tone: Monday eases in with planning, Wednesday pushes aggressive execution, Friday focuses on closing open loops
6. Newsletters, promos, and noise are collapsed into a "+N skipped" line -- only actionable items get full treatment

**Trading integration:** If you mention "levels", "trade", "6E", or "morning prep", the skill fetches Sierra tick data from your Windows PC in parallel and computes trading levels, bias analysis, and a full game plan. If you do not mention trading, it asks after showing the day plan.

## Key Features

- **Unified briefing:** Calendar, email, Slack, and project status in one output -- no app-switching
- **Rich action items:** Every actionable item includes context (who, what, history) and a concrete "Action:" line -- not just a subject line
- **Time-blocked day plan:** Specific sub-tasks mapped to time slots, not generic "deep work" labels
- **Week-ahead table:** See the next 4-5 days at a glance so nothing sneaks up on you
- **Noise filtering:** Newsletters, birthday emails, and digests are collapsed -- only real work surfaces
- **6E trading integration:** Full level analysis with VWAP, delta, RSI, support/resistance clusters, bias read, and game plan
- **Graceful degradation:** If Gmail, Slack, or the trading PC is unavailable, the briefing continues with available data and notes what was skipped

## Usage

```
morning coffee                # Full briefing without trading
plan my day                   # Same thing
morning coffee with levels    # Briefing + 6E trading levels and game plan
morning prep                  # Briefing + trading (auto-detected from keyword)
```

**Trigger phrases:** "morning coffee", "plan my day", "what's on today", "run morning coffee"

## Data Sources

| Source | What It Pulls | MCP Required |
|--------|--------------|-------------|
| Google Calendar | Today's events + next 4 days | Google Calendar MCP |
| Gmail | Unread/starred/important from last 24h | Gmail MCP |
| Slack | DMs and mentions from last 24h | Slack MCP |
| Project memory | Active statuses and blockers | Local filesystem |
| Sierra Chart (optional) | .scid tick data for 6E futures | Windows PC on local network |

---

Built with [Claude Code](https://claude.ai/code) as a slash command skill.
