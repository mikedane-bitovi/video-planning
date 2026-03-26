# Figma Just Gave AI Agents Write Access to Your Canvas

## Meta
- **Folder:** `videos/week2/figma-blog/`
- **Pillar:** Trend Reaction / AI Workflow Automation
- **Format:** Short (60–90 seconds)
- **Paired with:** Companion to Bitovi blog post — "Figma's MCP Just Gave AI Agents 'Write' Access. A First Look at What It Can Do."
- **Estimated length:** 60–90 seconds
- **CTA:** Read the full breakdown → bitovi.com/blog/figma-just-opened-the-canvas-to-agents

---

## Step 1: Research [x]
> Raw notes, sources, key facts, and context on the topic.
> Everything in the script should trace back here.

### Video Framing Note
This is a **visual companion** to the Bitovi blog post — not a review. The blog post covers the honest assessment (what works, what doesn't). The video's job is simply to show the workflow in action: what it looks like to call `use_figma`, what the canvas actually does, and how skills fit in. Viewers are blog readers who want to see the thing — not hear about it. Skip the pros/cons framing entirely.

### Sources
- Bitovi blog post (Ali Shouman, March 25 2026): https://www.bitovi.com/blog/figma-just-opened-the-canvas-to-agents.-heres-what-actually-happens
- Figma official announcement (Matt Colyer, March 24 2026): https://www.figma.com/blog/the-figma-canvas-is-now-open-to-agents/
- Figma developer docs — tools & prompts: https://developers.figma.com/docs/figma-mcp-server/tools-and-prompts/#use_figma

---

### The Announcement (March 24, 2026)
Figma announced that AI agents can now **write directly to the Figma canvas** through the MCP server via a new tool: `use_figma`. This is a meaningful step beyond read access — agents aren't just reading designs to generate code anymore; they're creating and editing design assets.

---

### What `use_figma` Actually Does
- General-purpose write tool via the Figma MCP server
- Agents can create, edit, delete, or inspect any object in a Figma file: pages, frames, components, variants, variables, styles, text, and more
- Before creating anything from scratch, the agent first checks the connected design system for existing components to reuse
- Works with: Claude Code, Cursor, Copilot in VS Code, Copilot CLI, Codex, Augment, Factory, Firebender, Warp
- **Currently free during beta** — will become usage-based paid
- Full seats: use anywhere. Dev seats: use in drafts only.
- 20kb output response limit per call; no image/asset support yet; custom fonts not yet supported

---

### What Actually Worked Well (from Bitovi's testing)
- **Building without a design system:** Agent correctly identified needed components, built base components first, then reused them to construct the full screen. The right instinct.
- **Code → Figma component:** Given an existing Button component with all its props and variants, it produced an accurate Figma component set with correct variants, styles, and clean layer naming.
- **Restructuring Figma Make output:** When explicitly told to create proper component sets with variables, it did — clean, structured, production-ready output that Figma Make alone wouldn't produce.
- **Cleaning up existing designs:** Swapped hardcoded hex values for proper tokens, replaced plain frames with real components from the library.

---

### Where It Needs Help (the critical caveat)
These aren't bugs — they're the nature of agentic tools:

- **Design system detection is NOT automatic.** You must explicitly tell it to use your linked design system every time. Without the instruction, it may use plain frames for some components and real ones for others — inconsistent.
- **Variables are NOT used by default.** Leave the agent to its own defaults and you get hardcoded hex values, hardcoded spacing, hardcoded typography. Ask it explicitly to use variables, and it does.
- **Non-determinism is real.** Same prompt, different results on different runs. The agent optimizes for the literal goal you gave it — not the implicit standards in your head.

---

### Skills: The Fix
A **skill** is a markdown file that packages a repeatable workflow into a reusable instruction set. Instead of re-explaining your conventions each time, a skill gives the agent a stable sequence of steps to follow.

- Every limitation above is addressable through a custom skill
- A skill can tell the agent: always check the design system first, always use variables, always create component sets
- Core foundational skill: `/figma-use` — all other skills build on this; gives agents a shared understanding of how Figma works
- How to invoke: `/skill-name` in Claude Code, slash command menu in Cursor, CLI in Codex
- Skills also enable **self-healing loops** — agent takes a screenshot of what it generated, checks it against what was asked, and iterates

**9 community skills launched with the announcement:**
- `/figma-generate-library`: Create new components from a codebase
- `/figma-generate-design`: Create new designs using existing components + variables
- `/apply-design-system`: Connect existing designs to system components
- `/sync-figma-token`: Sync design tokens between code and Figma variables with drift detection
- `/create-voice`: Generate screen reader specs (VoiceOver, TalkBack, ARIA)
- `/cc-figma-component`: Generate Figma components from a JSON contract
- `/rad-spacing`: Apply hierarchical spacing with variables and fallbacks
- `/edit-figma-design`: Orchestrate design workflows (Warp)
- `/multi-agent`: Run parallel workflows and implement designs (Augment Code)

---

### The Bigger Picture
Figma's framing: "Teams that figure out how to work with this are going to move significantly faster than those that don't."

The version most teams will actually use day-to-day isn't the raw `use_figma` tool — it's **the tool + a set of skills that encode your team's conventions**. That combination gets you design-system-aligned output you can actually use in production.

Bitovi has an active Figma partnership and early access — Ali tested this across 6 real test cases across 3 scenarios.

---

### Complementary Tool: `generate_figma_design`
Worth mentioning for context (not the focus of this video): `generate_figma_design` translates live HTML from web apps/sites into editable Figma layers. The two tools are complementary — when designs fall out of sync with code, `generate_figma_design` imports the latest UI; `use_figma` can then edit those designs using real components and variables.

---

## Step 2: Define the World [x]
- **Scenario:** Figma just shipped something meaningful — AI agents can now *write* to the canvas, not just read from it. The viewer is a developer or designer who's heard about this and wants to see what it actually looks like in practice.
- **Friction:** Before this, AI could read your Figma designs and generate code — but the canvas itself was read-only for agents. There was no way for an agent to create or modify design assets. That gap meant design work stayed manual even as everything else got automated.
- **Resolution:** `use_figma` closes that gap. And with Skills, you can encode your team's conventions so the agent builds the right way — using your design system, your variables, your components — every time.
- **Viewer takeaway:** "I know exactly what this capability looks like and how to start using it." The video stands alone — no blog post required to get value from it. Viewers who want the deeper honest breakdown can follow the link.

---

## Step 3: Mission Brief [x]
**Sections:**
1. **(Hook ~5s)** — Figma just gave AI agents write access to the canvas. That's new. Here's what it looks like.
2. **(Setup ~10s)** — Quick visual establish: a simple Figma file with a design system (buttons, basic components) on one side; Claude Code open and connected to the Figma MCP on the other. Split-screen format, vertical/short-friendly.
3. **(The tool in action ~25s)** — Type a prompt, hand it off to `use_figma`. Show the agent checking the design system, then components appearing on the canvas in real time. The moment of "oh, it actually just did that."
4. **(Skills ~20s)** — But if you want the agent to always follow your conventions — use your variables, check your library first — that's where Skills come in. Show a skill being invoked, brief result.
5. **(CTA ~5s)** — Link to the full write-up for the deep dive.

**Mission brief:** *We drop straight into a live Figma + Claude Code session — showing how to call `use_figma`, watch the canvas respond, then level up with a skill that makes the agent work your team's way.*

**Visual format note:** Vertical (9:16) short. Primary b-roll is a split-screen screen recording — Figma canvas left, Claude Code terminal right. Newly recorded, branded.

---

## Step 4: Value & CTA [x]
- **Insight bridge:** Most people have seen AI generate code *from* a Figma design. This flips it — the agent writes *back* to the canvas using your actual design system. That's the part nobody's talking about yet.
- **Secret sauce:** The Skills angle is the Bitovi moment. The raw `use_figma` tool is cool but inconsistent — that's what everyone else will show. Bitovi goes one level deeper: here's how you encode your team's conventions into a skill so the agent builds *your way*, every time. That's the insight that turns a fun demo into a real workflow.
- **CTA:** *"Want the honest breakdown of what works and what doesn't? We tested this across 6 real scenarios — link in bio."* → `bitovi.com/blog/figma-just-opened-the-canvas-to-agents`

---

## Step 5: Storyboard Notes [x]
Scene-by-scene breakdown with rough timing, b-roll plans, and visual element notes.
(Full storyboard lives in Google Slides — these are the notes to build from.)

**Scene 1 — Hook (~5s)**
- **Narration:** "Figma just gave AI agents write access to your canvas. Here's what that actually looks like."
- **On screen:** Cold open — jump cut straight into the split screen. Figma canvas visible, Claude Code terminal visible. No title card, no intro.
- **Visual notes:** Text overlay animates in: *"AI agents can now WRITE to Figma"*

**Scene 2 — Setup (~8s)**
- **Narration:** "I've got a simple Figma file here with a basic design system — a few components, some variables. Claude Code is connected to the Figma MCP server."
- **On screen:** Pan/zoom across the Figma file showing the components panel and a few existing elements. Quick cut to the Claude Code terminal showing MCP connected.
- **Visual notes:** Label overlays: *"Figma MCP connected"*, highlight ring around the components panel.

**Scene 3 — `use_figma` in action (~25s)**
- **Narration:** "I'm going to ask it to build me a new card component using what's already in the design system." *(types prompt)* "And there it is — it checked the library first, pulled the right components, and built it with proper auto layout and variables. No hardcoded values."
- **On screen:** Type the prompt into Claude Code → agent calls `use_figma` → watch the canvas populate in real time. Zoom in on the result: clean layer names, real components, variable fills.
- **Visual notes:** Speed ramp the agent "thinking" phase, slow down on the canvas populating. Highlight the layers panel showing real component names (not "Frame 47").

**Scene 4 — Skills (~20s)**
- **Narration:** "Now — by default the agent won't always follow your conventions. But that's what Skills are for. A skill is just a markdown file that tells the agent how your team works. I invoke it like this..." *(types `/figma-generate-design`)* "...and now it knows to always use your design system, your tokens, your standards."
- **On screen:** Brief flash of the skill markdown file (readable instructions), invoke the skill in Claude Code, show a clean canvas result.
- **Visual notes:** Text overlay: *"Skills = your rules, encoded once"*

**Scene 5 — CTA (~5s)**
- **Narration:** "We tested this across six real scenarios — link in bio for the full breakdown."
- **On screen:** Bitovi blog post headline visible. URL lower third.
- **Visual notes:** Bitovi logo bug bottom-right throughout entire video.

**Thumbnail concept:** Split screen — Claude Code terminal on the left with a prompt typed, Figma canvas on the right with a freshly generated component. Text overlay: *"AI just got write access to Figma"*

---

## Step 6: Script [x]
Full narration script.

---

Figma just redefined what's possible with AI Agents and design.

Before, agents could only read your designs. Now, they have "write" access.

Let's start with a totally blank canvas. I’m using Claude Code connected to the Figma MCP server. And I’ll give it a simple prompt asking it to build me a todo list. 

In order to do this, Claude will call the use_figma tool and provide it with instructions on what to build.

Give it a few seconds, and we have a fully polished design. And this isn't jsut a quick mockup, everything is labeled, organized into proper frames, and uses best practives. In other words, it's production ready. 

But it gets better. You aren't just limited to generic designs. You can actually have the agent build things that integrate directly with your existing design system.

Let's say you have a library with specific buttons, inputs, and brand variables already set up. To get the agent to use them, you just need to give it a little more detail in the prompt.

You can do this directly from the chat, or, even better, by using an Agent Skill. A skill is just a set of instructions that tells the agent: "Hey, when you build this, use our 'Primary Brand' color variable and pull from our 'Core Components' library.". Figma actually provides official skills that are great to use. 

When you provide that context, the agent stops guessing and starts building exactly the way your team does. It bridges the gap between a "cool AI demo" and your actual design workflow.

We took a deep dive into exactly how to set these skills up and tested it across several real-world scenarios. If you want to see the full breakdown and the results, check out the blog post linked below. 