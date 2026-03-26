# How to Get Claude Code to See Your Figma Designs

## Meta
- **Folder:** `videos/week2/claude-figma-mcp/`
- **Pillar:** AI Developer Enablement / AI Workflow Automation
- **Format:** Anchor Video
- **Estimated length:** 3–5 minutes
- **Paired with:** `claude-cascading` anchor video — this goes deeper on the Figma MCP piece; that video shows it as part of a full workflow
- **CTA:** "We can help you integrate AI into your SDLC" → bitovi.com/ai-for-software-teams

---

## Step 1: Research [ ]
> Raw notes, sources, key facts, and context on the topic.
> Everything in the script should trace back here.

### What the Figma MCP Does
The official Figma MCP server (hosted by Figma at `https://mcp.figma.com/mcp`) exposes a full suite of tools. Source: https://developers.figma.com/docs/figma-mcp-server/tools-and-prompts/

**Read / design-to-code tools (the core workflow):**
- **`get_design_context`** — The main tool. Pass a Figma frame/layer URL (or use your active selection with the desktop server), get back generated reference code. React+Tailwind by default, customizable to any framework. Can also reference components from your codebase via Code Connect.
- **`get_screenshot`** — Standalone screenshot of a selection. Helps preserve layout fidelity. Recommended to keep on unless you're worried about token limits.
- **`get_variable_defs`** — Returns variables and styles used in a selection (colors, spacing, typography). Good for token-aware code generation.
- **`get_metadata`** — Returns a sparse XML outline of a selection (layer IDs, names, types, positions, sizes). Useful for very large designs — agent can use this to break down a complex layout before calling `get_design_context` on individual parts.
- **`search_design_system`** — Searches connected design libraries for components, variables, and styles. Claude can find existing design system elements to reuse rather than inventing new ones.

**Code Connect tools (advanced — maps Figma components to codebase components):**
- **`get_code_connect_map`** — Returns a mapping of Figma node IDs → source code component locations. Claude uses this to know which existing component to *use* instead of generating from scratch.
- **`get_code_connect_suggestions`** — AI-suggested mappings between Figma nodes and code components.
- **`add_code_connect_map`** — Adds a new Figma → code component mapping.
- **`send_code_connect_mappings`** — Confirms Code Connect mappings after suggestions.
- **`create_design_system_rules`** — Generates a rules file agents can use to translate designs into codebase-aware frontend code.

**Write to canvas tools (brand new — beta):**
- **`use_figma`** — The general-purpose write tool. Create, edit, delete, or inspect any object in a Figma file: pages, frames, components, variants, variables, styles, text, and more. This is the big new capability — not just reading designs, but *writing* them back. Uses real Figma-native structure (Plugin API), not screenshots. Currently free during beta; will become usage-based paid. Requires a Full seat (not Dev seat). Source: https://developers.figma.com/docs/figma-mcp-server/write-to-canvas/
- **`generate_figma_design`** — Sends live UI from web apps as design layers directly into a Figma file. Remote only, select clients.
- **`create_new_file`** — Creates a new blank Figma Design or FigJam file.
- **`generate_diagram`** — Generates a FigJam diagram from Mermaid syntax (flowcharts, Gantt, sequence, state diagrams).

**Write to canvas — current limitations (beta):**
- 20kb output response limit per call
- No image/asset support yet
- Custom fonts not supported yet
- Beta quality — output may need review

### Seat Type Note
Dev seat holders can use all read tools (get_design_context, etc.). Write to canvas (`use_figma`) requires a Full seat.

### Prerequisite: Dev Mode
The Figma MCP requires **Dev Mode** to be enabled on the Figma file. Without it, the integration doesn't work at all — not a warning, a hard stop.

- Toggle: `Shift+D` (or the toggle in the toolbar, bottom center of the screen in the Figma desktop app)
- Dev Mode is available on all paid Figma plans
- Dev Mode is where all the developer inspection tools live — code snippets, layer specs, annotations, etc.
- Important: this is per-file, not global. You need Dev Mode enabled on the specific file you're linking Claude to.

### Two Ways to Configure the MCP

#### Option 1: `.mcp.json` at the project root ✅ Recommended for teams
Create a file called `.mcp.json` at the root of your project:

```json
{
  "mcpServers": {
    "figma-remote-mcp": {
      "transport": "http",
      "url": "https://mcp.figma.com/mcp"
    }
  }
}
```

**Why this is preferred:**
- Lives in the repo — everyone on the team gets the same MCP config automatically
- Can be checked into version control
- Works alongside other MCP servers (e.g. Atlassian for Jira) in the same file
- Per-project scope — different projects can have different MCP setups

#### Option 2: Terminal command (global Claude config)
```bash
claude mcp add --transport http figma-remote-mcp https://mcp.figma.com/mcp
```

**What this does:**
- Stores the server in Claude's global config (on your machine, not in the repo)
- Available in all Claude Code sessions, regardless of project
- Can't be shared via version control — each developer has to run it themselves
- Better if you want Figma access everywhere without needing a project-level config

### Auth Flow
Once configured (either way), type `/mcp` inside Claude Code. Claude Code will prompt you to authenticate with Figma via OAuth — it opens in the browser, same as a normal Figma login. After authorizing, the connection is established and persists.

### How to Get Figma Links
To give Claude access to a specific frame, component, or the whole file:
- **File link:** Just the URL in your browser when the file is open
- **Frame link:** Right-click a frame on the canvas → "Copy link to frame" (also `Ctrl/Cmd+L` with the frame selected)
- **Component link:** Same right-click → copy link — links directly to that component node
- **Tip:** Linking to specific frames is much better than the whole file — Claude gets focused context instead of the entire file's layout tree

### Key Talking Points
- MCP is the protocol that lets Claude "reach out" to external services like Figma
- Once connected, you just paste a Figma frame link into Claude Code and tell it what to do — no copy-pasting design specs, no screenshots
- `get_design_context` returns everything in one call: reference code + screenshot + component metadata + design tokens + Code Connect mappings
- Code Connect is the advanced layer — maps Figma components to actual codebase components so Claude uses your design system instead of generating from scratch
- This is the same setup used in the `claude-cascading` workflow — this video explains the Figma piece in depth

### Sources
- Figma MCP server: `https://mcp.figma.com/mcp`
- Claude Code MCP docs: `https://code.claude.com/docs/en/mcp`
- Figma Dev Mode guide: https://help.figma.com/hc/en-us/articles/15023124644247-Guide-to-Dev-Mode
- Related context: `videos/week2/claude-cascading/spec.md` (Scenes 4 & 5)

---

## Step 2: Define the World [x]
- **Scenario:** A designer or developer has a Figma file open — a design system with components, tokens, layouts. They want to explore it with Claude: understand what's in a component, extract token values, inspect a complex layout. Or they just want to ask Claude questions about the designs without switching contexts constantly.
- **Friction:** The only options right now feel clunky — screenshot and paste, or try to describe what you're looking at in the chat. Claude has no idea what's actually in the file. Every interaction requires you to manually relay information that's already sitting right there in Figma.
- **Resolution:** Figma's official MCP server gives Claude direct access to the design file. Copy a link to any frame, component, or element — paste it into Claude Code — and Claude can inspect it: see the structure, read the tokens, get a screenshot, understand the layout. No screenshots, no describing. Claude just looks.
- **Viewer takeaway:** This video isn't about building anything. It's about what's possible when Claude can see your Figma files directly — and how to set that up. Works for designers who want to explore their own designs with AI, and for developers who want to understand them before building.

---

## Step 3: Mission Brief [x]
Major sections and narrative arc — where does the viewer start and where do they end?

### Scenes

1. **Hook** (~10s): Caption text + narrator sets up the premise. Claude can read your Figma designs, inspect them in detail, and even create new ones. Here's how.

2. **What is MCP + Setup** (~45s): Quick explainer — MCP is how Claude talks to external services like Figma. One diagram, two sentences. Then straight into setup: enable Dev Mode in Figma (`Shift+D`, per-file, hard requirement), create `.mcp.json` at project root with the Figma server URL, type `/mcp` in Claude Code to authenticate. Done in under a minute.

3. **The Tools — Read** (~90s): Figma on the left, Claude Code on the right. Walk through the read tools one by one by actually using them. Copy a link to a component → paste it in → Claude calls `get_design_context` → show what comes back. Copy another link → call `get_variable_defs` → show the token output. Call `get_screenshot` → show the visual. Call `get_metadata` on a full page → show the lightweight structure overview. Each tool gets one demo beat — what it does, show it running, show the output.

4. **The Tools — Write** (~30s): `use_figma` just landed in beta. Claude can now write back to the canvas — create frames, components, variables, auto layout. Show it doing something simple in Figma. Quick note: requires a Full seat, free during beta. It's early, but the direction is clear.

5. **Getting Links Right** (~20s): Quick tip — right-click any frame or component on the canvas → Copy link to selection. Specific links give Claude focused context. The whole file URL works but gets noisy on large projects.

6. **CTA** (~20s): Mention the cascading video for anyone who wants to see this used in a full dev workflow (Jira + Figma + Claude building a feature). Code Connect callout. Bitovi CTA.

### Narrative arc
Viewer starts not knowing the Figma MCP exists. They get a quick setup (2 minutes), then watch Claude interact with real Figma designs — reading components, extracting tokens, inspecting layouts, and writing back to the canvas. They end knowing exactly what's possible and how to try it themselves.

**Mission brief:**
Set up the Figma MCP in Claude Code, then walk through every major tool live — copying design links, watching Claude inspect them, and showing what comes back. No coding. Just Claude and Figma talking to each other.

---

## Step 4: Value & CTA [x]
- **Insight bridge:** Most people don't know Figma ships an official MCP server. They're still screenshotting designs and pasting them into chat — or describing what they're looking at in words. That's a lossy, manual translation of information that's already structured in Figma. The MCP bypasses all of that: Claude reads the design directly, with full fidelity. And it's not just for developers — any designer or product person who wants to explore their designs with AI can use this.
- **Secret sauce:** The combination of tools is what's interesting. `get_design_context` gives you the full picture. `get_variable_defs` extracts just the tokens. `get_metadata` maps out a large file before you dive in. `use_figma` writes back to the canvas. Claude chains these together on its own — you just describe what you want and it figures out which tools to call. That orchestration is the real capability, not any single tool in isolation.
- **CTA (specific offer):** "If you want to see this used in a full development workflow — where Claude reads a Jira ticket, pulls the Figma designs, and implements the feature — we made a video on that too, link up here. And if you want help setting up Code Connect or integrating this into your team's process, that's what Bitovi does. Link in the description." → `bitovi.com/ai-for-software-teams`

---

## Step 5: Storyboard Notes [x]
Scene-by-scene breakdown with rough timing, b-roll plans, and visual element notes.
(Full storyboard lives in Google Slides — these are the notes to build from.)

**Screen layout throughout:** Figma desktop app on the left, Claude Code terminal on the right. Split-screen or cut between them. This is the visual throughline of the whole video.

---

### Scene 1 — Hook *(~10s)*
**Narrator:** On camera. Deliver the hook caption live.
**On screen:**
- [ ] Screen recording: Figma file open showing a component library / design system — browsing the canvas
**Notes:** Just establish the visual world — Figma, lots of components. Quick.

---

### Scene 2 — What is MCP + Setup *(~45s)*
**Narrator:** On camera. "To make this work, we need to connect Claude to Figma using something called MCP."
**On screen:**
- [ ] **Diagram needed:** Claude Code in center, arrow to Figma labeled "MCP". Simple. One sentence overlay: "MCP = how Claude talks to external services."
- [ ] Screen recording: Figma — enable Dev Mode (`Shift+D`). Callout: "Per-file. Required. Easy to miss."
- [ ] Screen recording: Run `claude mcp add --transport http figma-remote-mcp https://mcp.figma.com/mcp` in the terminal
- [ ] Callout annotation: "One command. Available in every project from now on."
- [ ] Mention `.mcp.json` as alternative for teams who want it in version control — one line, one second
- [ ] Screen recording: Type `/mcp` in Claude Code → OAuth browser popup → Figma connected
**Notes:** Move fast. This is setup, not the point of the video. Get through it in under a minute so we can get to the interesting part.
- [ ] **Asset needed:** MCP diagram graphic

---

### Scene 3 — The Read Tools *(~90s)*
**Narrator:** On camera to introduce, then light narration over screen. "Alright — Claude is connected to Figma. Here's what it can do."
**On screen — four tool demos, in sequence:**

**`get_design_context`:**
- [ ] Screen recording: Right-click a component in Figma → Copy link to selection
- [ ] Paste link into Claude Code: "What can you tell me about this component?"
- [ ] Show Claude calling `get_design_context` in the tool log
- [ ] Show the response — reference code, screenshot, token values. Pause on it briefly.
- [ ] Narrator: "One link. Claude gets the code structure, a screenshot, and all the design tokens."

**`get_variable_defs`:**
- [ ] Screen recording: Select a section of the design
- [ ] Ask Claude: "What variables and styles are used here?"
- [ ] Show Claude calling `get_variable_defs` → show the token output (colors, spacing, typography)
- [ ] Narrator: "All the design tokens, extracted directly."

**`get_screenshot`:**
- [ ] Ask Claude: "Can you take a screenshot of this frame?"
- [ ] Show `get_screenshot` called → image returned in Claude
- [ ] Quick beat — visual confirmation.

**`get_metadata`:**
- [ ] Navigate to a more complex page in Figma
- [ ] Ask Claude: "Give me an overview of what's on this page."
- [ ] Show `get_metadata` called → lightweight XML structure overview
- [ ] Narrator: "Useful for large files — Claude gets the lay of the land before diving into specific pieces."

**Notes:** This is the meat of the video. Each tool gets one clean beat: copy link → paste → tool fires → show output. Keep narration minimal — the tool calls and responses are the story.

---

### Scene 4 — The Write Tool: `use_figma` *(~30s)*
**Narrator:** On camera. "Now here's something new — Claude can also write back to the canvas."
**On screen:**
- [ ] Ask Claude to do something simple in Figma: "Create a new frame with a button component using our existing design system."
- [ ] Show `use_figma` firing — then show the result appear in Figma on the left
- [ ] Callout: "This just launched — it's in beta. Free for now, will be paid later. Full seat required."
**Notes:** Keep this brief. It's a teaser — the technology is new and the quality is beta. The wow moment is just seeing Claude write something back into Figma.

---

### Scene 5 — Frame Links *(~20s)*
**Narrator:** On camera. Quick tip.
**On screen:**
- [ ] Screen recording: Right-click a frame → "Copy link to selection"
- [ ] Callout: "Specific frame link = focused context. Whole file URL = noisy."
**Notes:** Could be woven into Scene 3 if pacing allows, but useful as its own beat so it doesn't get lost.

---

### Scene 6 — CTA *(~20s)*
**Narrator:** On camera, end card.
**Script beat:** Point to the cascading video for the full dev workflow. Code Connect mention. Bitovi CTA.
**On screen:**
- [ ] End card with CTA: `bitovi.com/ai-for-software-teams`
- [ ] Link/card to `claude-cascading` video

---

### B-Roll & Asset Checklist
- [ ] **Prepare:** Figma design file with a component library — enough variety to demo multiple tools (components, tokens, a complex layout page)
- [ ] Screen recording: Figma canvas — browsing component library
- [ ] Screen recording: Enabling Dev Mode in Figma (`Shift+D`)
- [ ] Screen recording: `.mcp.json` being created
- [ ] Screen recording: `/mcp` auth flow — Figma OAuth → connected
- [ ] Screen recording: Right-click frame → Copy link to selection
- [ ] Screen recording: `get_design_context` — paste link → tool fires → response shown
- [ ] Screen recording: `get_variable_defs` — token output
- [ ] Screen recording: `get_screenshot` — image returned
- [ ] Screen recording: `get_metadata` — structure overview of a complex page
- [ ] Screen recording: `use_figma` — Claude writes something back to the Figma canvas
- [ ] **Create:** MCP diagram graphic (Claude Code ↔ Figma)
- [ ] End card graphic with CTA link

---

## Step 6: Script [x]

This is a great foundation. The technical explanation is solid, but we can make the "hook" punchier and smooth out the transitions between the "How-to" and the "Tool deep-dive" to keep the viewer's momentum going.

Here is a refined version of your script, optimized for a tech-demo or YouTube format.

---

## **[INTRO: The Hook]**

Did you know Claude can look at your Figma designs and use them to build pixel-perfect UIs? 

Actually, it goes much deeper than just "looking." Using Figma’s **Model Context Protocol (MCP)** tools, Claude can inspect the underlying structure of your files, learn your design system, and—with the new `use_figma` tool—it can even build designs directly on the canvas for you.

Let's take a look.

On the left, I have a component library for an app I’m building. On the right, Claude Code is ready to roll. Let’s grab a link to one of these components, and ask Claude to inspect it. 

Now with the right setup, Claude doesn't just see a picture; it gets the full design context — detailed CSS, spacing variables, metadata or even a screenshot.

I can also ask Claude to create a new design directly on the Figma canvas, and within seconds, I have a new component that fully integrates into my existing design system.

All of this is possible with MCP. For those new to the term, MCP is the standard for how AI agents communicate with apps like Figma, Slack, or Jira. It’s essentially a bridge that gives Claude a "toolbox" to interact with your Figma designs. And it only take a few seconds to set up.

The first thing you’ll need is a Figma plan with Dev Mode access. Look at the bottom of your Figma window for the mode switcher, if you can toggle Dev Mode on you’re good to go, otherwise you'll need to upgrade your plan.

Once dev mode is enabled, in Claude Code, run the one-line config command to add the Figma MCP server to your global config. This makes Figma available across all your projects.

If you're working with a team, you can also define this in a `.mcp.json` file within a code repo, which helps if you're usign this to devleop an app.

One more thing to note. There are actually two ways to run Figma's MCP, either directly from the Figma desktop app, or using a remote url hosted by Figma. In this video we'll use the Figma hosted version as it tends to have the latest tools enabled first.

Once configured, just type `/mcp` in Claude Code. It’ll prompt you to log into Figma via your browser, and just like that, you’re connected.

Now that we’re linked, let’s look at the "toolbox" Claude is actually using to interact with Figma. Understanding these helps you prompt more effectively.

First we have the read tools: 

get_design_context is the workhorse. Give it a link, and it returns the design as clean code. It defaults to React and Tailwind, but you can ask for any framework.

get_variable_defs  pulls your design tokens—colors, spacing, and typography—ensuring the code Claude writes actually matches your theme.

search_design_system let's Claude look through your designs to learn more about what's available.

Then there's the write tools:

use_figma is the newest and it's a game-changer. Claude can create, edit, or delete layers directly on your canvas. Imagine describing a new layout and watching Claude generate the first draft in Figma while you watch.

And if you need a fresh start, create_new_file can spin up a new Figma Design or FigJam file instantly.

The best part? You rarely need to call these by name. Just tell Claude what you want to achieve, and it will pick the right tool for the job.

The last think I'll touch on before we wrap up is code connect. Code connect is Figma's way of directly mapping the components you have in Figma, to the components you've implemented in your codebase. 

If you want to bridge the gap between design and production entirely, keep an eye on the get_code_connect_suggestions tool. It helps you map Figma components directly to your production code.

Code Connect is the "holy grail" for ensuring the code Claude generates is 100% aligned with your designs.

If you need help setting up Code Connect or integrating AI workflows into your team's design-to-code pipeline, that’s exactly what we do at Bitovi. As official Figma partners, we help teams bridge the gap between design and engineering using the latest AI tech.

Check the link in the description to learn more about our AI enablement services.

And if you like this content, we have another video on how to "one-shot" a Jira ticket using Figma and Atlassian's MCPs

Thanks for watching — now go build something cool!

