# Expert Knowledge System

Turn any expert's content (YouTube channels, podcasts, docs) into a personalized, cited knowledge base via NotebookLM.

## How It Works

1. **Add an expert** — scrape their YouTube channel, create a NotebookLM notebook, bulk-load videos
2. **Query** — ask questions with citations traced to exact transcript passages
3. **Scale** — repeat for any expert in any domain

## Project Structure

```
experts/                          # Expert registry
  experts.json                    # Master list of all loaded experts
  huberman/                       # Per-expert folder
    config.json                   # Expert metadata + notebook ID
    videos.json                   # Scraped video list
    queries/                      # Saved Q&A outputs
.claude/skills/notebooklm/       # NotebookLM skill (installed)
```

## Adding a New Expert

```bash
# 1. Scrape channel
python3 .claude/skills/notebooklm/scripts/load_channel.py scrape \
  --channel "https://www.youtube.com/@ChannelHandle" \
  --output experts/<slug>/videos.json

# 2. Create notebook
notebooklm create "<Expert Name>"

# 3. Load videos
python3 .claude/skills/notebooklm/scripts/load_channel.py load \
  --videos experts/<slug>/videos.json \
  --notebook <notebook-id> \
  --count 200 --concurrency 20
```

## Prerequisites

- `nlm` CLI: `uv tool install notebooklm-mcp-cli`
- `notebooklm-py`: `pip install "notebooklm-py[browser]"`
- Auth: `nlm login` and `notebooklm login` (browser-based Google login)

## Loaded Experts

See `experts/experts.json` for the current registry.
