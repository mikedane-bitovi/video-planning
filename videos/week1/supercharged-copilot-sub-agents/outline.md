# Outline: Supercharged Copilot with Sub Agents

## Overview
- **Format:** A1 — The Workflow Demo
- **Pillar:** AI Developer Enablement
- **Target Length:** ~3–4 minutes
- **Primary Audience:** Both (senior devs + engineering leaders who vibe code)
- **Goal:** Viewer leaves understanding what sub agents are, why they matter, and how to think about designing workflows with them — with enough curiosity to try it and enough trust to think about who built the system behind it.

---

## Scenes

### Scene 1 — The Hook (Demo First, No Intro)
- **Narration:** You just used AI to build an entire app. Now comes the part nobody talks about — how do you know if this code is actually any good? You could dump all of it into a single Copilot chat and ask it to review everything... but if there's enough code, you're going to run into the context wall. And even if you don't, one agent trying to review security, performance, AND code quality all at once is not a great review. So instead — this.
- **On Screen:** VS Code workspace with the vibe-coded todo app — enough files to feel substantial. Presenter narrates over it briefly, then cut to the chat view: main agent spawning 3 sub agents simultaneously. All three collapsible tool calls appear at once and start running in parallel.
- **Notes:** No intro, no name — open straight on the scenario. The three-agents-at-once visual is the hook. Let it breathe for a second before moving on.

### Scene 2 — What Just Happened (What Are Sub Agents)
- **Narration:** What you just saw is sub agents. Instead of one agent trying to hold all your code in its head at once, the main agent splits the work — spawning independent agents to review security, performance, and code quality separately. Each one gets its own isolated context, does its job, and reports back.
- **On Screen:** High-level diagram — main agent in the center, three sub agents branching off of it, results flowing back. No deep internals yet — just the shape of the relationship. Think org chart, not flowchart.
- **Notes:** This is the "aha, I get the concept" moment. Keep it visual and simple. The deeper architecture comes in Scene 5 — don't front-load it here.

### Scene 3 — How to Enable It
- **Narration:** To use sub agents you just need one thing: make sure the `runSubagent` tool is enabled in your Copilot settings. That's it. Once it's on, the agent decides when to use it — you don't have to do anything special in your prompts.
- **On Screen:** Quick screen recording — VS Code settings, finding and toggling the `runSubagent` tool on.
- **Notes:** Fast, functional. 15 seconds max.

### Scene 4 — Spawning a Single Sub Agent
- **Narration:** Let's back up and look at the simplest case first. Say our todo app needs some documentation written — we want a sub agent to go through the codebase and generate a README. You can invoke one explicitly by typing `#runSubagent` in the chat, or you can just tell the agent what to do and it'll figure out that a sub agent makes sense. Watch what happens.
- **On Screen:** Demo clip — presenter types a prompt along the lines of "Use a sub agent to go through this codebase and write a README." The sub agent tool call appears in chat. Then: expand the tool call to reveal the prompt the main agent generated for the sub agent.
- **Notes:** The big moment here is expanding the tool call to show the generated prompt. Pause on it. "The agent just wrote instructions for another agent." This is the architecture made visible.

### Scene 5 — The Architecture (How It Really Works)
- **Narration:** Here's what's happening under the hood. The main agent writes a prompt — it's doing prompt engineering on your behalf. The sub agent spins up with a completely clean context: no conversation history, no prior context, just that task. It does its work using its own tool calls, and when it's done it either returns a text summary or writes results to a file. The main agent picks that up and keeps going.
- **On Screen:** Deeper architecture diagram — the full single sub agent flow beat by beat: main agent reasons → generates prompt (reference the actual prompt we saw in Scene 4) → sub agent spawns with isolated context → sub agent runs its own tool calls → returns result as text or file → main agent integrates and continues. Each step animates or highlights as it's narrated.
- **Notes:** This is the "now you understand what you just saw" scene — the Scene 4 demo is the proof, this diagram is the explanation. Should feel like the pieces clicking into place. The two diagrams (Scene 2 high-level, Scene 5 detailed) build on each other intentionally.

### Scene 6 — Back to Parallel: The Code Review
- **Narration:** Now let's go back to where we started. Three sub agents running at the same time — this is parallel execution. The main agent launches all of them at once, each one goes off and does its focused review, and then — this is the key thing — the main agent waits for all of them to finish before it does anything. All the results come back, get fed into the main context, and then the agent synthesizes a verdict.
- **On Screen:** Return to the original parallel demo clip. Show all three sub agent tool calls completing. Then show the main agent producing the final synthesized code review output.
- **Notes:** Explicitly call out the timing: all must finish before results flow back. The synthesized output is the payoff — show it, don't just imply it happened.

### Scene 7 — Workflow Patterns
- **Narration:** Once you understand the basic mechanics, you can start designing real workflows. Here's what's possible — and what isn't.

  **Valid patterns:**
  You can run a single sub agent, then spawn a batch in parallel, then another single one after. You can loop — run a reviewer, apply the fixes, run the reviewer again. You can go as many rounds as you need. Mix and match.

  **What's not possible — interleaving:**
  Say you launch two sub agents in parallel. One finishes early. You might think — great, let's kick off the next task while the other one is still running. You can't. The main agent has to wait for both to finish before it can do anything. It's all-or-nothing per batch.

  **What's also not possible — recursive nesting:**
  Sub agents can't spawn their own sub agents. One level deep, that's the ceiling. It keeps things predictable — you always know who's in charge of what.
- **On Screen:** A series of workflow diagrams:
  1. Valid: one → three parallel → one (sequential batches)
  2. Valid: loop pattern — sub agent A → sub agent B → back to sub agent A if needed
  3. Invalid: two parallel sub agents, one finishes, a third spawns before the second completes — show this crossed out or marked as ✗
  4. Invalid: sub agent trying to spawn its own sub agent — also ✗
- **Notes:** The invalid patterns are just as important as the valid ones. Showing what breaks helps people design correctly and builds credibility. Each diagram should be visually distinct — valid patterns in one style, invalid patterns clearly marked.

### Scene 7.5 — Subtle Mid-Video CTA (Integrated)
- **Narration:** If you're thinking about how to actually roll this out on your team — what workflows make sense, what guardrails to put in place — that's exactly the kind of thing we help teams figure out. Link in the description.
- **On Screen:** Presenter on green screen, one line of text lower-third or subtle URL overlay. Flows naturally out of the workflow patterns discussion.
- **Notes:** This should feel like a peer giving a genuine recommendation, not an ad break. One sentence, don't stop the momentum. Keep the energy up going into the next section.

### Scene 8 — Custom Agents as Sub Agents (Brief)
- **Narration:** One more thing worth knowing: you can define custom agents specifically designed to act as sub agents — a security reviewer wired up to the right MCP tools, a planner that's restricted to read-only access, each with its own model. When the main agent delegates, it can hand off to one of these specialists instead of a generic instance. We'll go deep on this in a future video.
- **On Screen:** Quick flash of an `.agent.md` file with `user-invocable: false` set — enough to show it's real without explaining it fully.
- **Notes:** One breath, no deep dive.

### Scene 9 — Quick FAQs
- **Narration:** Before we wrap, a few things people always ask.
- **On Screen:** Rapid-fire text cards or lower-thirds, one per beat:
  - "Do sub agents use premium requests?" → No.
  - "Do they see your conversation history?" → No — only what the main agent sends them.
  - "Are they slower?" → About the same. Maybe a bit.
  - "Do they inherit your tools and MCP servers?" → Yes — and you can restrict that with custom agent files.
  - "Do they follow your copilot-instructions?" → Honestly? Inconsistent. Sometimes yes, sometimes no. Worth testing in your setup.
- **Notes:** The honest last answer builds more trust than a clean "yes." End on that note — it signals intellectual honesty.

### Scene 10 — Consequence + CTA
- **Narration:** Sub agents are how you go from AI that helps you write code to AI that can actually manage complexity at scale. If you want to know where your team stands on this stuff — or you want help actually building it out — there's an AI Readiness Assessment and team training options in the description. And if you're wondering what's really going on with context windows and why any of this matters — that's the next video.
- **On Screen:** Presenter on green screen. End card: two CTAs (AI Readiness Check + Bitovi training) + brief tease for the context window short.
- **Notes:** The context video tease should feel like a natural next step, not a promo.

---

## B-Roll & Asset Checklist
- [ ] Screen recording: VS Code workspace with vibe-coded todo app (enough files to look substantial)
- [ ] Screen recording: 3 parallel sub agents spawning and running simultaneously in chat view
- [ ] Screen recording: Single sub agent invoked via prompt (README generation scenario)
- [ ] Screen recording: Expanding a sub agent tool call to reveal the generated prompt (key moment)
- [ ] Screen recording: VS Code settings — enabling the `runSubagent` tool
- [ ] Screen recording: Main agent synthesized output from the 3 parallel sub agents (show the actual output)
- [ ] Diagram: High-level "hub and spoke" — main agent center, sub agents branching off (Scene 2)
- [ ] Diagram: Detailed single sub agent flow — main agent → generates prompt → isolated context → tool calls → result → integrate (Scene 5)
- [ ] Diagram: Parallel sub agent flow — main agent → 3 simultaneous → all complete → converge → synthesize (Scene 6)
- [ ] Diagram: Workflow patterns (Scene 7):
  - Valid: sequential batches (one → parallel three → one)
  - Valid: loop (A → B → back to A)
  - Invalid ✗: interleaving (one of two parallel finishes, third spawns before second completes)
  - Invalid ✗: recursive nesting (sub agent spawning a sub agent)
- [ ] Quick shot: `.agent.md` file with `user-invocable: false`
- [ ] End card graphic: Two CTAs (AI Readiness Check + Bitovi training) + context video tease

---

## Suggestions
- The mid-video CTA (Scene 7.5) works best if it comes exactly at the moment viewers are most impressed — right after seeing workflow patterns, before the FAQ cooldown. That's when the "I want this for my team" thought is most active.
- Consider whether the FAQ rapid-fire works as on-screen text cards (faster, more visual) or straight narration. Text cards with the Q appearing first, then the answer, could be punchy.
- The "AI writing a prompt for another AI" moment in Scene 4 is worth a zoom or pause — it's the most visually interesting single beat in the entire video.

## Open Questions
- What's the exact README/single-sub-agent scenario — should it be writing a README, or something more visual like adding JSDoc comments or generating a test file?
- Should the workflow patterns diagram match the existing architecture diagram aesthetic, or can it be a simpler hand-drawn/whiteboard style?
