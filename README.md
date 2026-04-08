# Expert Knowledge System

**Load any expert's entire YouTube catalog into a searchable knowledge base. Query it with citations traced to exact transcript passages.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Built with Claude Code](https://img.shields.io/badge/Built%20with-Claude%20Code-blueviolet)](https://claude.ai/claude-code)
[![NotebookLM](https://img.shields.io/badge/Powered%20by-NotebookLM-4285F4)](https://notebooklm.google.com)

---

395 Huberman Lab episodes. One command to load. Every answer cited to the exact episode and passage.

But this is not a Huberman tool. It is a general-purpose system for turning **any** expert's content into a queryable, cited knowledge base — from the terminal.

## What This Does

```
YouTube Channel                NotebookLM                    Your Terminal
+-----------------+     +----------------------+     +------------------------+
| 300+ videos     | --> | Indexed & searchable | --> | Cited answers          |
| Any expert      |     | Google's AI handles  |     | Traced to exact        |
| Any domain      |     | transcript parsing   |     | transcript passages    |
+-----------------+     +----------------------+     +------------------------+
      SCRAPE                   LOAD                        QUERY
   (no API key)          (parallel upload)           (structured citations)
```

**Three capabilities:**

1. **Bulk load** -- Scrape an entire YouTube channel and load 300+ videos into NotebookLM. One command. No API keys. No manual entry.

2. **Cited answers** -- Ask any question. Every recommendation traces to the exact episode and transcript passage. Verifiable, not hallucinated.

3. **Deep expert plans** -- Generate comprehensive protocols, playbooks, and guides synthesized across hundreds of episodes. See the [example below](#example-30-day-dopamine-reset-from-395-huberman-episodes).

## Quick Start

### 1. Install

```bash
# NotebookLM CLI (for queries)
uv tool install notebooklm-mcp-cli

# NotebookLM Python client (for bulk loading)
pip install "notebooklm-py[browser]"
playwright install chromium

# Authenticate (opens browser for Google login)
nlm auth login
notebooklm login
```

### 2. Scrape and Load

```bash
# Scrape all videos from a channel (no API key needed)
python3 .claude/skills/notebooklm/scripts/load_channel.py scrape \
  --channel "https://www.youtube.com/@hubermanlab" \
  --output experts/huberman/videos.json

# Create a NotebookLM notebook
notebooklm create "Andrew Huberman - Health"

# Load 300 videos in parallel (~75 seconds)
python3 .claude/skills/notebooklm/scripts/load_channel.py load \
  --videos experts/huberman/videos.json \
  --notebook <notebook-id> \
  --count 300 \
  --concurrency 20
```

### 3. Query

```bash
nlm notebook query <notebook-id> \
  "What does Huberman recommend for sustaining deep focus for 4+ hours?" --json
```

Every answer comes back with `[N]` citations pointing to the exact source and passage.

## Example: 30-Day Dopamine Reset from 395 Huberman Episodes

This is a real output from querying 395 Huberman Lab episodes loaded into NotebookLM. The system synthesized a structured 30-day plan with phase-by-phase protocols, supplement stacks, exercise programming, and emergency craving tools -- all cited to specific episodes.

> **Phase 1 (Days 1-10): Withdrawal & Foundation**
>
> Execute 6 daily anchors: morning sunlight (5-30 min within 60 min of waking),
> cold exposure (1-3 min, gives sustained 250% dopamine increase for hours),
> exercise (<75 min), NSDR/Yoga Nidra (restores dopamine reserves by up to 65%),
> sleep protocol (lights out by 10-11 PM), no caffeine before 90 min after waking.
>
> **Phase 2 (Days 11-20): Rewiring**
>
> Layer on gratitude protocol, journaling (15-20 min continuous writing about
> stressful experiences -- proven to improve immune function), procedural memory
> visualization. Introduce focus supplements intermittently: L-Tyrosine 500mg,
> Alpha-GPC 300mg, every 3rd or 4th session only.
>
> **Phase 3 (Days 21-30): Consolidation**
>
> Intermittent reward training -- randomly skip caffeine, work out without music,
> attach dopamine to effort not outcome. Meditation 5-13 min daily to train
> prefrontal cortex. Careful re-introduction of one eliminated behavior on Day 28.

Full plan: [`experts/huberman/30-day-dopamine-reset-plan.md`](experts/huberman/30-day-dopamine-reset-plan.md)

That file includes daily schedules, supplement dosing tables, weekly exercise splits, emergency craving protocols, and weekly check-in templates -- all derived from the expert's own research-backed recommendations across 395 episodes.

## Add Any Expert

The same three commands work for any YouTube channel.

### Lenny's Podcast (Product Management)

```bash
python3 .claude/skills/notebooklm/scripts/load_channel.py scrape \
  --channel "https://www.youtube.com/@LennysPodcast" \
  --output experts/lenny/videos.json

notebooklm create "Lenny's Podcast - Product"

python3 .claude/skills/notebooklm/scripts/load_channel.py load \
  --videos experts/lenny/videos.json \
  --notebook <notebook-id> \
  --count 200 --concurrency 20
```

Then query: *"What are the most effective user onboarding frameworks discussed across all episodes?"*

### More Examples

| Expert | Channel | Domain | Use Case |
|--------|---------|--------|----------|
| Alex Hormozi | `@AlexHormozi` | Business | Offer creation, pricing, lead generation |
| Lex Fridman | `@lexfridman` | AI / Science | Research landscape, expert perspectives |
| Ali Abdaal | `@aliabdaal` | Productivity | Systems, tools, time management |
| My First Million | `@MyFirstMillionPod` | Startups | Business ideas, market analysis |
| Naval Ravikant | `@naval` | Wealth / Philosophy | Mental models, decision frameworks |
| Y Combinator | `@ycombinator` | Startups | Fundraising, product-market fit |

Any channel with substantial content becomes a queryable expert knowledge base.

## How It Works

### Architecture

```
                    +-------------------+
                    |   YouTube         |
                    |   InnerTube API   |
                    +--------+----------+
                             |
                    scrape (no API key,
                    pure Python stdlib)
                             |
                             v
                    +-------------------+
                    |  videos.json      |
                    |  [{id, title,     |
                    |    url, ...}]     |
                    +--------+----------+
                             |
                    load (async, 20
                    concurrent requests)
                             |
                             v
                    +-------------------+
                    |   Google          |
                    |   NotebookLM     |
                    |                   |
                    |  Indexes videos,  |
                    |  parses trans-    |
                    |  cripts, builds   |
                    |  knowledge graph  |
                    +--------+----------+
                             |
                    query via nlm CLI
                    (cited responses)
                             |
                             v
                    +-------------------+
                    |  Cited Answers    |
                    |  [1] -> Episode   |
                    |  [2] -> Passage   |
                    |  [3] -> Timestamp |
                    +-------------------+
```

### Key Technical Details

- **YouTube scraping** uses the InnerTube browse API directly. No `yt-dlp`, no API keys, no external dependencies. Pure Python stdlib with continuation token pagination.

- **Parallel loading** via `notebooklm-py` async client. 20 concurrent requests loads 200 videos in ~75 seconds. For channels with 300+ videos, the system automatically splits across multiple notebooks (NotebookLM has a 300-source limit per notebook).

- **Citation resolution** replaces `[N]` markers with clickable wikilinks that jump to the exact cited passage in source transcripts. Anchor IDs are MD5-hashed for stability. ~96% resolution rate.

- **Claude Code integration** via the `.claude/skills/notebooklm/` skill. Workflows handle the full pipeline: scrape, load, query, resolve citations.

## Project Structure

```
HealthwithHubber/
  .claude/
    skills/
      notebooklm/
        SKILL.md                         # Skill definition
        scripts/
          load_channel.py                # Scrape YouTube + bulk-load into NotebookLM
          resolve_citations.py           # Replace [N] with linked wikilinks
          import_sources.py              # Import sources as local files
          extract_passages.py            # Extract cited passages from Q&A
          backfill_fulltext.py           # Fetch full transcripts
        workflows/
          youtube-channel.md             # Channel loading workflow
          ask.md                         # Q&A with citations workflow
          import.md                      # Source import workflow
          auth.md                        # Authentication workflow
  experts/
    experts.json                         # Registry of all loaded experts
    huberman/
      config.json                        # Expert metadata + notebook IDs
      videos.json                        # 395 scraped videos
      30-day-dopamine-reset-plan.md      # Example deep plan output
      queries/                           # Saved Q&A outputs
  CLAUDE.md                              # Project documentation
  README.md                              # This file
```

## Prerequisites

| Requirement | Install | Purpose |
|-------------|---------|---------|
| Python 3.10+ | -- | Script runtime |
| `nlm` CLI | `uv tool install notebooklm-mcp-cli` | Query notebooks, list sources |
| `notebooklm-py` | `pip install "notebooklm-py[browser]"` | Create notebooks, bulk-load videos |
| Playwright | `playwright install chromium` | Browser auth for notebooklm-py |
| Google account | -- | NotebookLM access (free) |

**No YouTube API key required.** Scraping uses YouTube's InnerTube API directly.

**No paid APIs for loading.** NotebookLM is free. The `nlm` CLI and `notebooklm-py` are open-source tools that interface with it.

## Limits

- **300 sources per notebook.** Channels with more videos are automatically split across multiple notebooks.
- **Processing time.** After upload, NotebookLM indexes each video server-side. Allow a few minutes before querying.
- **Some videos may fail** if they are private, deleted, or region-locked. Errors are logged to `/tmp/channel-load-errors.json`.

## License

MIT
