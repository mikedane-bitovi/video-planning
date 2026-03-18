# Research: Subagents: Save Your Context Window

## Topic Summary
This video is a short-form follow-up to "Supercharged Copilot with Sub Agents" that drills into one specific, high-value use case: using sub agents to protect the main agent's context window. The core insight is that sub agents shine when a task requires ingesting a large amount of raw information that isn't valuable to keep — the sub agent does the heavy lifting (reading, parsing, analyzing), returns only the distilled result, and the main agent's context stays clean. This makes it both a practical tip for developers and a compelling visual demo.

## Key Facts & Concepts

- **Context windows have a finite size.** Every token loaded into a conversation — file reads, tool outputs, intermediate results — consumes context space. Once it's used, it's used. Large context = slower responses, higher token cost, and risk of hitting the limit entirely.
- **Reading a large file inline is expensive.** When the main agent reads a large log file directly, the entire file content lands in the main context window. If the file is thousands of lines, that's thousands of tokens consumed — even if only 3 facts from it are actually relevant.
- **Sub agents run in isolated context windows.** A sub agent spins up clean: no conversation history, no prior context. Whatever it reads or processes stays inside that bubble. Only the final response comes back to the main agent.
- **The sub agent acts as a distillation layer.** The sub agent reads the whole thing → understands it → returns a compact summary. The main agent only ever sees the summary. This is the fundamental pattern.
- **This is especially powerful for log analysis.** Log files are a canonical example: potentially huge, mostly noise, with a handful of meaningful events buried inside. Perfect candidate for sub agent parsing.
- **The contrast is visually demonstrable.** Side-by-side windows — one using sub agents, one not — make the context consumption difference immediately visible. You can literally see the context fill up (or not).
- **This generalizes beyond logs.** Any task that requires "going wide" to gather information before synthesizing an answer benefits from this pattern: reading large files, scraping documentation, searching through many files, pulling in API responses, etc.
- **Related concept: the sub agent as a "read-only worker."** The sub agent does research — files, searches, context — and hands back findings. It never needs to hold that context in the main window. This is a mental model worth naming explicitly.

## How It Works (Step-by-Step)

### Without Sub Agents (The Problematic Path)
1. User asks: "What errors happened in the last hour according to the logs?"
2. Main agent reads `app.log` directly — all 5,000 lines land in the main context window.
3. Main agent processes the content and answers.
4. 5,000 lines of log content are now sitting in the main context, consuming tokens for every subsequent message — even though only 3 error entries were relevant.

### With Sub Agents (The Better Path)
1. User asks: "What errors happened in the last hour according to the logs?"
2. Main agent writes a focused prompt for a sub agent: "Read app.log and return only the error events from the last hour, as a brief summary."
3. Sub agent spins up with an isolated context window. It reads the full 5,000 lines — in its own bubble.
4. Sub agent returns a compact summary: "3 errors found: [timestamp] NullPointerException in auth.ts, [timestamp] DB connection timeout, [timestamp] rate limit exceeded."
5. Only those ~50 tokens land in the main context. The 5,000 lines are gone.
6. Main agent continues with a clean, focused context.

## Interesting Angles & Talking Points

- **The log file is a relatable pain point.** Every developer has had the experience of a huge log dump clogging up a chat. It's immediately recognizable.
- **The before/after contrast makes the abstract concrete.** Showing the context fill up (or not) side by side turns "context management" from a concept into something you can see happening in real time.
- **The sub agent doesn't just save space — it saves precision.** When the main agent isn't drowning in raw log data, it can reason better about what actually matters. Less noise = better answers.
- **This is also a cost argument.** More tokens = more cost. Teams running AI workflows at scale care about this. The sub agent pattern isn't just elegant — it's economical.
- **The "distillation layer" mental model.** Naming the pattern explicitly — "the sub agent is a distillation layer between raw data and your main context" — gives viewers a framework they can apply broadly.
- **Teases the broader context management topic.** This video is naturally positioned as a bridge: it shows sub agents in a specific context, but the underlying issue (context window management) is a whole topic unto itself.

## Known Limitations & Gotchas

- **The sub agent's summary is only as good as its prompt.** If the main agent writes a vague task prompt ("look at the logs"), the sub agent might return irrelevant information or miss the point. This is worth acknowledging briefly.
- **Sub agents still consume tokens.** The sub agent itself has a context window — if the log file is enormous (millions of lines), the sub agent could hit its own limits. The sub agent doesn't have unlimited capacity; it just keeps that cost out of the main window.
- **There's a slight latency overhead.** Spawning a sub agent takes a moment. For a quick one-shot question on a small file, it may not be worth it. The pattern pays off on large files or complex research tasks.
- **The runSubagent tool must be enabled.** (Covered in the previous video — worth a brief callback but no need to re-explain.)

## Reference Material

- [VS Code Sub Agents docs](https://code.visualstudio.com/docs/copilot/agents/subagents) — context isolation is described explicitly; result delivery options (text vs. file) are documented
- [Agents concepts — context isolation](https://code.visualstudio.com/docs/copilot/concepts/agents#_subagents) — background on why and how context isolation works
- Previous video: "Supercharged Copilot with Sub Agents" — established the foundational concepts; this video builds on that foundation without re-explaining the basics

## Suggestions

- **Format: S3 — The Before/After.** The side-by-side demo is a natural Before/After structure. Old way: context gets bloated. New way: context stays clean. The contrast is self-evident and visually punchy.
- **Lead with the "after" shot.** Open on the sub agent result — a clean, compact summary — before showing how the context-bloated version compares. Mirrors the S1 format's "result first" energy.
- **Use a real-looking but controlled log file.** Generate a fake `app.log` with ~500–1000 lines (visible line count in the editor), with a handful of buried error entries. Enough to look substantial; controlled enough that the demo is reliable.
- **Make the context fill visual.** When showing the "without sub agents" path, the viewer should be able to see the file content streaming into the chat window and taking up space. The visual impact is the argument.
- **The sub agent's returned summary should be clean and readable.** Something like a 3–5 line summary of what it found — enough to show it's useful, compact enough to make the point about token efficiency.
- **Keep narration tight.** At 60–90 seconds, every line needs to earn its place. The demo should do most of the explaining. Narration frames it, doesn't describe everything that's visible.
- **Optional closer: "This works for more than logs."** One line that generalizes the pattern — documentation, large config files, search results, API dumps — plants the seed without adding runtime.

## Open Questions

1. **What format exactly?** S3 (Before/After) feels like the right fit given the side-by-side demo structure, but S1 (The Reveal) format — leading with the working result — is also viable. Do you want to lead with the clean sub agent outcome and work backward, or show the problem first?

- S3 is fine 

2. **How big should the log file be visually?** Big enough to feel painful (scrolling forever, thousands of lines), but not so large the demo itself becomes slow. Any preference on the size/volume to convey?

- It's ~1000 lines

3. **What does the main task look like?** What question should the agent be asking of the logs — "what errors happened in the last hour"? "Summarize what went wrong today"? Something more specific or more open-ended?

- It's something like "read through the entire log file and surface any logs that seem out of place". This is just for demo purposes

4. **Side-by-side or sequential?** Show both windows at the same time, or show the non-sub-agent path first, then the sub-agent path? Side-by-side is more visually immediate; sequential gives more room to explain each path.

- I think show both at the same time. Although maybe we want to start with another example?? IDK we need to frame the problem which is doing exploration or extra "work" muddies the main agent's context window, then we need to demonstrate clearly how the subagent helps with that. So either we show one example of the main agent's context getting muddied then we dive into the demo, or somehow just incorporate the demo for this. 

5. **Narration style:** Should this feel more like a quick tip / "did you know" energy, or more of a mini-explainer? At 60–90 seconds it's tight either way — just calibrating the tone.

- I think a mini-explainer

6. **Callback to the previous video:** How explicitly should this reference "Supercharged Copilot with Sub Agents"? A quick "if you haven't seen the sub agents video, link below" line? Or assume the viewer already knows sub agents and don't mention it?

- No need to reference it I think

7. **Ending CTA:** For a short, the CTA is typically just intrigue/tease, not a hard ask. Should we tease the full context window video? Point to the sub agents deep-dive? Or include a soft Bitovi mention?

- We need a clear CTA for them to click the link in the description
