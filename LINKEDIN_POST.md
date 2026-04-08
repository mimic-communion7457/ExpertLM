# LinkedIn Post Draft

---

I built a system that lets me talk to any expert's entire body of work.

Here's the problem: Andrew Huberman has 395 YouTube episodes. Lex Fridman has 400+. Ali Abdaal, Hormozi, Lenny's Podcast -- hundreds more.

The knowledge is there. But nobody has time to watch 2,000+ hours of content to find the one protocol that actually applies to them.

So I built ExpertLM.

One command loads 300+ YouTube episodes from ANY expert into Google NotebookLM. No manual entry. No API keys. No copy-pasting URLs one by one.

Then you ask questions -- and every answer comes with citations traced back to the exact episode and transcript passage.

Here's what it looks like in practice:

I loaded all 395 Huberman Lab episodes and asked:
"How do I overcome dopamine addiction?"

In 30 seconds, I got a research-backed answer pulling from 15+ episodes, with every claim linked to the specific moment in the specific video where Huberman said it.

Then I had Claude Code build me a complete 30-day dopamine reset plan -- synthesized from ALL 395 episodes. Not a summary. A personalized, day-by-day protocol with:

-- 3 phases (withdrawal, rewiring, consolidation)
-- Exact supplement dosages and timing
-- Exercise protocols with specific durations
-- Emergency toolkit for when cravings hit
-- Weekly check-in templates

All cited. All verifiable. All from the expert's own words.

The system is expert-agnostic. Same pipeline works for:
-- Health: Huberman, Rhonda Patrick, Peter Attia
-- Business: Hormozi, Naval, Y Combinator
-- Product: Lenny's Podcast, Shreyas Doshi
-- AI/Tech: Lex Fridman, Andrej Karpathy
-- Productivity: Ali Abdaal, Cal Newport

The stack:
-- YouTube scraping via InnerTube API (zero dependencies, no API key)
-- Google NotebookLM for knowledge indexing and cited retrieval
-- Claude Code for orchestration and plan generation
-- Python scripts for bulk loading (300 videos in ~3 minutes)

I open-sourced the entire thing.

Link in comments.

The future of learning isn't watching more content.
It's building systems that extract exactly what you need, when you need it.

---

#AI #NotebookLM #ClaudeCode #Productivity #LearningSystem #OpenSource #BuildInPublic #ExpertKnowledge #Huberman #Health
