# Outline: Subagents: Save Your Context Window

## Overview
- **Format:** S3 — The Before/After
- **Pillar:** AI Developer Enablement
- **Target Length:** ~60–90 seconds
- **Primary Audience:** Both (senior devs + engineering leaders)
- **Goal:** Viewer walks away with one concrete mental model — "use a sub agent when you need to bring in a lot of context that the main agent doesn't actually need to hold." Ends with a click on the description link.

---

## Scenes

### Scene 1 — What's a Context Window (Quick Setup)
- **Narration:** Every conversation you have with an AI agent has a context window — think of it as the agent's working memory. Every message you send, every file it reads, every tool it runs — all of that goes into the window. The bigger it gets, the more the agent has to juggle. And it has a limit.
- **On Screen:** Clean animated diagram: a simple box labeled "Context Window." Messages and file reads drop in one by one, filling it up. A token counter ticks upward. Simple, fast, readable.
- **Notes:** This is a 10–12 second setup — just enough to establish the mental model. Don't over-explain. The viewer doesn't need to know what a token is; they just need to understand "it fills up."

### Scene 2 — The Compaction Problem
- **Narration:** Here's where it gets painful. You ask the agent to go research something — read a log file, dig through the codebase, whatever. It does all that work and gives you the answer. Great. But now all of that raw research is still sitting in the window. Every message you send from here is competing with thousands of lines of stuff the agent doesn't need anymore. And when the window gets too full — it compacts. The AI summarizes everything down to make room. You lose detail. The agent loses focus. Responses get slower and fuzzier. It's one of the most common reasons a long chat session starts to feel like the agent is losing the thread.
- **On Screen:** Animation continuing from Scene 1 — the context window now filling fast with a log file read, intermediate tool calls, raw output. Then the compaction animation: the contents get visually compressed/squished, some content fades out or gets blurred. A label: "Compaction: detail is lost." Presenter narrates on green screen.
- **Notes:** This is the emotional core of the problem frame. Make the compaction feel bad — slow, lossy, annoying. Viewers who have experienced this will recognize it immediately. This earns the "aha" when the sub agent fix lands.

### Scene 3 — What's a Sub Agent (Quick Definition)
- **Narration:** This is where sub agents come in. A sub agent is a completely separate AI instance that spawns off your main agent. It has its own isolated context window — its own bubble. It goes off, does all the messy research, loads in whatever it needs. And when it's done, all of that disappears. The main agent only gets back the exact answer it was looking for.
- **On Screen:** Simple animation: the main agent's context window on one side, a sub agent bubble spinning off of it on the other. The sub agent's bubble fills with raw data/research. Then it pops — leaving only a small, clean result that flows back into the main context. Clear and visual.
- **Notes:** 10 seconds max. This is a bridge scene — it names the solution before the demo shows it. Viewers unfamiliar with sub agents get a working mental model; viewers who already know get a quick reminder framed around context isolation specifically.

### Scene 4 — Before: Live Demo Without Sub Agents
- **Narration:** Here's what that looks like in practice. We have a log file — about a thousand lines. We ask the agent to read through all of it and surface anything that looks out of place.
- **On Screen:** Screen recording — the main agent (no sub agents) receives the prompt: "Read through the entire log file and surface any logs that seem out of place." The agent reads `app.log` (~1000 lines). The raw log content streams visibly into the chat window — a wall of output flooding the context.
- **Notes:** Let the content stream in. The visual weight of a thousand lines of logs in the chat window is the argument. Viewer should feel the bloat before the narration labels it.

### Scene 5 — After: Same Task With a Sub Agent
- **Narration:** Now the same prompt — but this time the agent uses a sub agent. It goes off, reads the entire log file in its own isolated context. And it comes back with just the three things that actually mattered.
- **On Screen:** Side-by-side or cut between the two Copilot windows. Sub agent version: the main chat stays minimal. The sub agent tool call appears, runs, and returns a clean 3–5 line summary of the anomalous log entries. Expand the sub agent call briefly to show it did the reading inside its own bubble.
- **Notes:** The side-by-side contrast here is the payoff — the two chat windows should look dramatically different. The sub agent's compact result should be readable on screen. Don't dwell on the internals; let the clean context speak for itself.

### Scene 6 — The Insight (Generalize + CTA)
- **Narration:** The sub agent did all the reading. Your main context only saw the answer. No compaction. No noise. Whatever you ask next — it's working with a clean window. This works for logs, documentation, large config files, any situation where you need to bring in a lot of context to find a little bit of signal. If you want help building this kind of workflow for your team, link in the description.
- **On Screen:** Presenter on green screen. Clean close. Lower-third or end card with the description link CTA.
- **Notes:** Hit the compaction callback ("No compaction. No noise.") — it ties the opening concept closed. The generalization line plants the seed that this is broadly applicable. CTA is a peer recommendation, not a pitch.

---

## B-Roll & Asset Checklist
- [ ] Animation: Context window box filling up with messages/file reads, token counter ticking up (Scene 1)
- [ ] Animation: Context window fills rapidly with research/log output → compaction step — contents compress, some detail fades (Scene 2)
- [ ] Animation: Main agent context + sub agent bubble spinning off → sub agent fills with data → bubble pops → clean result flows back to main (Scene 3)
- [ ] Screen recording: Demo A — main agent reads `app.log` (~1000 lines) directly; raw log content streams into chat window
- [ ] Screen recording: Demo B — same prompt, sub agent version; sub agent tool call appears, runs, returns compact 3–5 line summary; main chat stays minimal
- [ ] Screen recording: Sub agent tool call expanded briefly to show reading happened inside it (isolation made visible)
- [ ] Screen recording or still: Side-by-side of both chat windows showing the contrast in context state
- [ ] Fake `app.log` file: ~1000 lines of realistic-looking log output with a handful of buried anomalous entries (errors, unusual patterns) so the sub agent's summary is clean and readable on screen
- [ ] Presenter on green screen: Scenes 2 and 6 (compaction narration + close + CTA)

---

## Suggestions
- **The two opening animations are load-bearing.** The context window fill (Scene 1) and the compaction squish (Scene 2) set up everything that follows. Viewers who've never thought about context windows need Scene 1; viewers who have will feel Scene 2 in their bones. Both are worth real production investment.
- **The visual contrast is everything.** Scenes 2 and 3 should feel dramatically different. The side-by-side cut (or split screen) of the two chat windows is the single most important editorial moment in the video.
- **The fake log file matters.** It should look real — timestamps, log levels, realistic app event names — but be controlled so the demo is reliable. The anomalous entries should be obvious enough that the sub agent's summary reads as genuinely useful.
- **Don't explain sub agent mechanics.** This video assumes the viewer has seen or knows what sub agents are. No need to define them — jump straight into using them.
- **The compaction callback in Scene 6 ties the video closed.** "No compaction. No noise." is a two-word payoff to the opening concept — make sure the edit keeps that line.

## Open Questions
- **None — all answered.** Ready to proceed to scripting once this outline is approved.
