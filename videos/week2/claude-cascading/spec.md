# One-Shot a Feature with Claude Code, Jira & Figma

## Meta
- **Folder:** `videos/week2/claude-cascading/`
- **Pillar:** AI Developer Enablement / AI Workflow Automation
- **Format:** Anchor Video
- **Estimated length:** 3–5 minutes
- **CTA:** "We can train your team to use this workflow" / "We can help you integrate this into your SDLC"
- **Series:** Part 3 — previously produced for GitHub Copilot and WindSurf

---

## Step 1: Research [x]
> Raw notes, sources, key facts, and context on the topic.
> Everything in the script should trace back here.

### The Workflow
The video demonstrates Bitovi's "Cascading AI Enablement" workflow for one-shotting feature implementation using Claude Code:

1. **Jira ticket is the source of truth** — A properly structured Jira story contains a user story description *and* embedded Figma design links. The ticket is the single source of context for the AI.
2. **MCP servers bridge the tools** — Claude Code is configured with the Atlassian (Jira) MCP and Figma MCP servers. These let Claude read a live Jira ticket and pull images directly from Figma without any copy-paste by the developer.
3. **The "magic prompt" drives the agent** — Bitovi's open-source `generate-feature` prompt chain (from `bitovi/ai-enablement-prompts`) guides Claude Code step-by-step through the process: fetch ticket → extract Figma links → download designs → write code.
4. **One command, one shot** — Developer opens Claude Code, pastes the prompt (or references the GitHub file), provides the ticket number, and Claude Code does the rest: reads Jira, fetches Figma, generates code.

### Key Assets / Sources
- **AI Enablement Prompts:** [github.com/bitovi/ai-enablement-prompts](https://github.com/bitovi/ai-enablement-prompts)
  - 100 stars, 55 forks — real community traction
  - The key prompt: `writing-code/generate-feature/generate-feature.md`
  - Usage pattern: paste prompt into Claude Code + set `{TICKET_NUMBER}` = your Jira issue
  - Prompt chain capabilities: fetch Jira ticket data, extract Figma URLs, download designs, organize context, implement feature

- **Figma MCP (official):** `claude mcp add --transport http figma-remote-mcp https://mcp.figma.com/mcp`
- **Atlassian MCP (official):** `claude mcp add --transport http atlassian https://mcp.atlassian.com/v1/mcp`
- **Claude Code docs:** [code.claude.com/docs/en/mcp](https://code.claude.com/docs/en/mcp)

### Claude Code vs. Other AI Agents (Why This Is Interesting)
- **Terminal-first, highly autonomous.** Claude Code runs as a CLI tool (`claude`) — not embedded in an IDE. This gives it a different feel: more like a pair programmer with shell access than an autocomplete assistant.
- **MCP setup is CLI-native.** Unlike VS Code (which uses `mcp.json`), Claude Code uses `claude mcp add --transport http [name] [url]` commands. Simple, scriptable, easy to demo. In this case though we want also show `.mcp.json` at the root dir of the project as being the preferred way to configure these. So we can mention both approaches, but the .mcp.json is easier to "show" in the video.
- **OAuth auth flow built-in.** Running `/mcp` inside Claude Code triggers browser-based OAuth for Atlassian and Figma — seamless, no token management.
- **Context-aware, long-horizon tasks.** Claude Code is optimized for multi-step agentic tasks that span multiple files — ideal for "implement this ticket" prompts.

### Jira Ticket Structure (Important Setup Detail)
For the workflow to succeed, the Jira ticket needs to be structured well:
- Clear user story or task description
- Figma design links embedded in the ticket body (not just attached)
- Enough acceptance criteria for the AI to know when it's done
- The MCP reads the full ticket including description, comments, and metadata

### Series Context
This is the same workflow Bitovi has already demonstrated for:
- **GitHub Copilot** (VS Code) — uses `mcp.json` config, agent mode in chat
- **WindSurf** — uses its own agent interface

Claude Code brings a different flavor: terminal-based, scriptable, more agentic. The core workflow is identical — the differentiation is in *how* Claude Code handles autonomy and MCP configuration. Remember we want to mention how you can configure the mcp with a command, but reccomend the `.mcp.json` file as it's per repo and can be checked into version control.

### Demo Feature: Priority Labels + Filter
The demo app is a simple todo list. The feature being added:
- When creating or editing a task, the user can select a priority: **Low**, **Medium**, or **High**
- Each task displays a colored priority badge (e.g. green/yellow/red pill)
- A filter bar at the top lets the user view tasks by priority level
- Touches multiple layers: data model (new field), UI component (badge), filter state logic — impressive multi-touch change from a single prompt

**Why this works for the demo:**
- Immediately understandable — zero explanation needed
- Non-trivial to implement cleanly — a human dev would spend 45–60 min getting all the pieces right
- Highly visual — the before/after in the Figma designs is obvious
- Figma-friendly — colored badge component + filter tabs are easy to mockup as clean static designs

### Numbers Worth Mentioning
- `ai-enablement-prompts`: 100 ⭐ / 55 forks — real organic adoption
- Bitovi's best-performing video (Jira + GitHub + Copilot): 18.2k views — proof this topic resonates

---

## Step 2: Define the World [x]
- **Scenario:** A developer on a team opens their Claude Code terminal. Their ticket board has a story ready — a feature request from the design team, spec'd out in Figma. Normally, this is where the choreography begins: open Jira, read the ticket, open Figma, extract the designs, switch back to the code editor, manually describe what needs to be built. Then prompt the AI. Then realize you forgot a detail. Switch back to Figma. Repeat.
- **Friction:** The source of truth is scattered. The *what* lives in Jira. The *how it looks* lives in Figma. The *code* lives in the repo. Getting an AI agent to bridge all three without losing context requires a lot of manual handholding — which defeats the purpose.
- **Resolution:** Connecting Claude Code to the official Figma and Atlassian MCP servers, combined with Bitovi's open-source prompt chain, collapses that coordination into a single command. Claude reads the ticket, follows the Figma links embedded in it, downloads the designs, and implements the feature — all in one shot. MCP is the bridge; Bitovi's refined prompts are the guide that directs the AI through the best possible workflow.
- **Viewer takeaway:** They should feel like they just saw something that genuinely changes how they think about AI in their workflow. Not "cool autocomplete" — but "my tools are finally talking to each other." And they should want to set this up immediately.

---

## Step 3: Mission Brief [x]
Major sections and narrative arc — where does the viewer start and where do they end?

### Scenes

1. **Set the scenario** — Lay out the starting point: here's a Jira ticket with a feature to build, here are the Figma designs that define what it should look like, and here's Claude Code. The goal: implement the feature without ever leaving the terminal.
2. **The demo** — Run it live. Claude Code reads the ticket, follows the Figma links, pulls the designs, and implements the feature. Show the finished result. Let the viewer process what just happened.
3. **MCP setup** — Show the `.mcp.json` file at the project root. Two servers: Figma + Atlassian (official MCPs). Explain why `.mcp.json` is the preferred approach — it's per-repo and can be checked into version control so the whole team shares the same config.
4. **Figma Dev Mode** — Explain that the Figma MCP requires Dev Mode to be enabled on the Figma file. Quick callout — easy to miss, blocks the whole workflow if you skip it.
5. **The magic prompt** — Walk through the `generate-feature` prompt from `bitovi/ai-enablement-prompts`. What does it actually do? It's not magic — it's Bitovi distilling the best step-by-step workflow for this task, refined across real projects. Show how to reference it or paste it in.
6. **Jira ticket structure** — What makes a ticket work well with this workflow: Figma design links embedded in the description (not just attached), clear acceptance criteria, explicit scope. The more information in the ticket, the better the output.
7. **CTA** — Bitovi can help your team set this up and integrate it into your SDLC.

### Narrative arc
Viewer starts thinking "that looks cool but complex." By the end they realize the complexity is already solved — two config lines, one open-source prompt, and a properly written ticket is all it takes.

**Mission brief:**
We set the scene — a Jira ticket, Figma designs, and a blank Claude Code terminal — watch the feature get built in real time, then break down the three things that make it work: the MCP setup, Figma Dev Mode, and Bitovi's open-source prompt chain.

---

## Step 4: Value & CTA [x]
- **Insight bridge:** Connecting Claude Code to Jira and Figma via MCP is actually the easy part — anyone can do it in a few minutes. The harder part is knowing how to *guide the agent through the workflow correctly* once it has all that context. Just pointing Claude at a Jira ticket and saying "build this" isn't enough. You need a well-sequenced prompt that tells it how to read the ticket, what to extract from the designs, and how to structure the implementation. That's the prompt engineering layer — and it's where most teams get stuck. Bitovi has done that research, refined it across real client projects, and open-sourced it. The `ai-enablement-prompts` repo has 100 stars and 55 forks because people recognize it works.
- **Secret sauce:** The `generate-feature` prompt chain from `bitovi/ai-enablement-prompts`. It's the difference between "Claude kind of got it" and a clean, first-pass implementation that respects your codebase conventions. It's not magic — it's Bitovi's accumulated experience with this workflow, packaged as a reusable open-source prompt.
- **CTA (specific offer):** "Bitovi can help you integrate AI into your software development lifecycle — we've already done this with real engineering teams. Link in the description." → [bitovi.com/ai-for-software-teams](https://www.bitovi.com/ai-for-software-teams)

---

## Step 5: Storyboard Notes [x]
Scene-by-scene breakdown with rough timing, b-roll plans, and visual element notes.
(Full storyboard lives in Google Slides — these are the notes to build from.)

---

### Scene 1 — Set the Stage *(~30 sec)*
**Narrator:** Green screen, on camera. "I have a todo list app, I have a new feature I want to add, I have a Jira ticket, and I have Figma designs. Here's what I'm working with."
**On screen:** Cut between:
- [ ] Screen recording: the current todo list app (bare, no priority labels)
- [ ] Screen recording: the Jira ticket for the priority labels feature
- [ ] Screen recording: the Figma designs (before they exist — create these first)
**Notes:** Keep this tight. No jargon, no MCP mention yet. Just: here's the starting point.

---

### Scene 2 — The Magic Prompt + Demo *(~60–90 sec)*
**Narrator:** On camera. "At Bitovi, we've been developing what I like to call magic prompts — open-source prompts we've refined to guide AI agents through specific workflows. Let me show you what I mean."
**On screen:**
- [ ] Screen recording: Claude Code open in terminal
- [ ] Narrator pastes the GitHub link to `writing-code/generate-feature/generate-feature.md` and adds the Jira ticket number — that's the entire input
- [ ] Hit enter — now just watch
- [ ] Time-lapse/sped-up: Claude Code working — show it hitting the Jira MCP, parsing the ticket, following Figma links, downloading designs, writing code
- [ ] Narrator lightly narrates what's happening: "It pulls the Jira ticket... finds the Figma links... grabs the designs... and now it's writing the code."
- [ ] Show finished feature running in the app: priority badges on tasks, filter bar working
**Notes:** Keep narration minimal during the demo — let it breathe. The visual is the story. Don't over-explain yet.

---

### Scene 3 — Jira Ticket Structure *(~30–45 sec)*
**Narrator:** On camera. "Alright, want to know how to set this up yourself? Let's start with the Jira ticket."
**On screen:**
- [ ] Screen recording: the Jira ticket used in the demo — annotated or zoomed in
- [ ] Highlight: the feature requirements written in Gherkin-style format (Given/When/Then) — mention this as a recommended practice
- [ ] Highlight: the Figma design links embedded in the ticket description (not just attached)
- [ ] Callout: in Figma, you can grab links to individual frames — show how to copy a frame link and drop it into the ticket description for precise context
**Notes:** The key message: the more explicit and complete the ticket, the better the output. Figma links in the body (not attachments) is a gotcha worth calling out.

---

### Scene 4 — Figma Dev Mode + What is MCP *(~30–45 sec)*
**Narrator:** On camera. Quick pivot to Figma, then a beat to explain MCP.
**On screen:**
- [ ] Screen recording: Figma file — show enabling Dev Mode (toggle in top right)
- [ ] Callout: Dev Mode is what activates the Figma MCP server — if it's off, the integration doesn't work
- [ ] Diagram: Simple visual explaining MCP — "Claude Code in the middle, arrows out to Jira, Figma, etc. MCP is the protocol that lets Claude reach out and talk to these services."
**Notes:** Keep the MCP explanation to 1–2 sentences + diagram. Don't get lost in the weeds. The diagram does the heavy lifting.
- [ ] **Asset needed:** MCP diagram graphic (Claude Code ↔ Jira, Claude Code ↔ Figma, connected via MCP)

---

### Scene 5 — Claude Code: MCP Config + Auth *(~45 sec)*
**Narrator:** On camera. "Now let's set this up in Claude Code."
**On screen:**
- [ ] Screen recording: `.mcp.json` file at root of project — show the Figma + Atlassian MCP entries
- [ ] Mention: you can also run `claude mcp add --transport http ...` as a terminal command, but `.mcp.json` is the recommended approach because it lives in the repo and the whole team shares the same config
- [ ] Screen recording: inside Claude Code, type `/mcp` — show the authentication flow for both Figma and Atlassian (OAuth browser popup)
- [ ] Show both services as "connected" after auth
**Notes:** The `.mcp.json` per-repo angle is a nice differentiator from the VS Code version — worth a quick callout.

---

### Scene 6 — The Bitovi Magic Prompt *(~30–45 sec)*
**Narrator:** On camera. "So what is this magic prompt actually doing?"
**On screen:**
- [ ] Screen recording: `github.com/bitovi/ai-enablement-prompts` — navigate to `writing-code/generate-feature/`
- [ ] Show the prompt file — walk through the key steps it instructs Claude to take: fetch ticket → extract Figma links → download designs → organize context → implement
- [ ] Callout: 100 stars, 55 forks — people are using this
- [ ] Back to Claude Code: show the actual input — just the GitHub link to the prompt + `{TICKET_NUMBER}` = your Jira ID
**Notes:** Frame this as: "We did the prompt engineering research so you don't have to." The open-source nature is a trust signal — nothing hidden.

---

### Scene 7 — CTA *(~15 sec)*
**Narrator:** On camera, end card visible.
**Script beat:** "If you want to bring this workflow into your team — the setup, the prompt engineering, how to structure your Jira tickets so AI can actually use them — that's exactly what Bitovi does. We've done this with real engineering teams. Link in the description."
**On screen:**
- [ ] End card with CTA link: `bitovi.com/ai-for-software-teams`

---

### B-Roll & Asset Checklist
- [ ] Screen recording: baseline todo app (no priority labels)
- [ ] Screen recording: Jira ticket for priority labels feature (Gherkin format + Figma links)
- [ ] **Create:** Figma file — designs for priority label feature (badge component + filter bar)
- [ ] Screen recording: Figma Dev Mode toggle enabled
- [ ] Screen recording: Claude Code terminal — full demo run (sped up)
- [ ] Screen recording: finished feature in app (priority badges + filter working)
- [ ] Screen recording: `.mcp.json` file with Figma + Atlassian config
- [ ] Screen recording: `/mcp` auth flow in Claude Code (Figma + Atlassian OAuth)
- [ ] Screen recording: `bitovi/ai-enablement-prompts` GitHub repo + prompt file
- [ ] **Create:** MCP diagram graphic (Claude Code ↔ Jira + Figma via MCP)
- [ ] End card graphic with CTA link

---

## Step 6: Script [x]

---

**[HOOK — caption text]** (6s)

I just one-shotted a Jira ticket with Claude Code, and not only did it work, it also matched my Figma designs exactly.

---

**[SCENE 1 — SET THE STAGE]** (30s)

To show this off, imagine I'm a developer working on this todo list app. And let's say my product manager specs out a new feature in a Jira ticket. They want to add priority labels for each todo item, Low, Medium and High.

The Jira ticket links out to Figma designs that show exactly what this should look like, including some options for filtering and sorting. 

Now, normally I might sit down and build this myself, but let me show you how I can get Claude Code to read the Jira ticket, inspect the designs, and write the code in just a couple minutes.

---

**[SCENE 2 — THE MAGIC PROMPT + DEMO]** (1:10 m)

All I need to do is open Claude Code, tell it the ID of the Jira ticket, and paste in the link to one of Bitovi's "magic prompts" (this is the secret sauce, more on these later)

*[demo runs — narrate lightly over the action]*

Let's watch as Claude starts working. 

First thing it does is call out to Jira to get the ticket info. It reads the ticket and figures out what we want it to build. 

Then, in parses out the Figma links and looks at each design to figure out what the new feature should look like.

Agents like Claude are surprisingly good at understanding designs, and Figma provides a really nice interface for this which we'll dive into a bit later. 

Once it understands the requirements and designs, it's ready to start implementing.

When it's done, we can test out the new feature and, if nescessary, ask Claude to fix any issues. 

So all we had to do was tell it the ID of the Jira ticket and paste in a link to a prompt and within a few minutes we have a working feature!

Let me also show you something cool. This app has a Storybook component library, and look — Claude added the new priority badge component right in there too. It didn't just write the feature, it fit it into how the codebase was already organized.

So that's the basic workflow, now let's dive into exactly how it works.

---

**[SCENE 3 — JIRA TICKET STRUCTURE]** (45 s)

We'll start with the Jira ticket, because how you write it directly affects what Claude produces.

Here's the ticket I used. The requirements are written in Gherkin format — using Given, When, Then language.

It's a structured way to describe behavior, and AI agents respond really well to it because it's explicit and unambiguous.

The other important thing: the Figma design links are embedded right in the ticket description. In Figma, you can right-click any frame and copy a direct link to just that frame. The more you can point Claude at exactly the right part of the design, the better the output.

Keep in mind that the better you can outline what you want to build in the ticket and designs, the better Claude will do at implementing it. This sounds simple, but it's often the biggest problem when working with agents, not being speciic enough about what you want.

---

**[SCENE 4 — FIGMA DEV MODE + MCP]** (45 s)

So Jira is looking good, now over in Figma — there's one thing you can't skip. Down at the bottom of the screen there's a toggle for Dev Mode. This is what allows developer tools like Claude to access information about your designs. In order for Claude to read your designs you need to have an account with access to dev mode, if you can toggle this on, you're good to go.

Enabling Dev Mode also turns on an MCP server for Figma — MCP is at the heart of this entire workflow so let's take a second to talk about it. 

MCP or Model Context Protocol, is how AI Agents like Claude can easily talk to external services like Jira or Figma. Using MCP, Claude can ask Jira for a specific ticket, or Figma for information about what a design looks like. 

Each 3rd party service has a set of "tools" that Claude can use to accomplish it's task. And setting up an MCP connection is easier than you might think.

---

**[SCENE 5 — CLAUDE CODE: MCP CONFIG + AUTH]** (40 s)

There are actually a couple options for configuring MCP in Claude Code, but the most straightforward is to create an `.mcp.json` file at the root of your project — here's what it looks like with both the Figma and Atlassian MCP servers configured. We just need to include the URL to the mcp server along with a descriptive name.

You can also add servers with a terminal command, but the `.mcp.json` approach is better for teams because it lives in the repo and everyone gets the same config automatically.

Once that file is there, open Claude Code, type `/mcp`, and it walks you through authenticating with each service — Atlassian for Jira, Figma for designs. It opens in the browser, same as a normal login. After that, Claude has access to both and stays connected.

---

**[SCENE 6 — THE BITOVI MAGIC PROMPT + CTA]** (1:10 m)

Now that Claude is connected to Jira and Figma, the final piece is that "magic prompt" we discussed earlier. 

Bitovi has an entire repository of battle tested prompts on GitHub that you can just paste into the chat window for Claude to read.

In this case there's a prompt in the `writing-code` folder called `generate-feature`. This prompt walks the agent through this workflow:

Reading the Jira ticket, inspecting the designs, understanding the codebase and finally implementing the feature pixel perfect, with minimal changes. 

This same basic workflow can used in a variety of circumstances. The central idea is that you connect Claude to external services via MCP, and then write a detailed orchestration prompt to handle the implementation (or whatever it is you want it to do). 

All of your favorite apps are investing in MCP. Slack, Canva, Notion, Stripe, and databases like Postgres and Supabase. You can imagine the possibilites here. 

And that's exactly the research we've been doing at Bitovi. We work with AI agents across real teams, figure out what actually works, and put it into prompts anyone can use.

If you want to bring this into your own team's workflow — the setup, the prompt engineering, structuring your Jira tickets so AI can actually use them — that's exactly what we help with. Link in the description.

That's gonna do it for me, thanks for watching, and keep innovating!
