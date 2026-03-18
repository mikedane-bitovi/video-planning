# Research: Supercharged Copilot with Sub Agents

## Topic Summary
Sub agents in GitHub Copilot (VS Code) allow the main agent to delegate focused subtasks to independent AI instances that run in isolated context windows. The core value proposition is context management: instead of every intermediate step polluting the main conversation, a sub agent does the work and returns only a summary. This is a practical power-user feature that makes complex, multi-step AI workflows significantly more efficient — and it's available today in VS Code.

## Key Facts & Concepts

- **Enablement:** The `runSubagent` tool must be enabled in Copilot settings for the main agent to invoke sub agents. Users can also enable the `agent` tool in `.prompt.md` file frontmatter.
- **Context isolation:** Each sub agent runs in its own context window. It does NOT inherit the main agent's conversation history, instructions, or in-progress context — it only receives the task prompt the main agent writes for it.
- **Synchronous by default:** The main agent waits for all sub agent results before proceeding. Sub agent findings typically inform what happens next, so the flow is sequential at the orchestration level even when sub agents run in parallel.
- **Parallel execution:** VS Code can spawn multiple sub agents simultaneously. The main agent launches them all at once, waits for all to complete, then synthesizes the results. There is no "interleaving" — you cannot react to one sub agent finishing while another is still running.
- **One layer of nesting only:** Sub agents cannot spawn their own sub agents. The hierarchy is strictly: main agent → sub agents. No deeper nesting is supported.
- **Result delivery:** Sub agents can return results as text (summary back to the main context) or by writing to a file (temp file or workspace file), which the main agent can then read.
- **The main agent writes the prompt:** The main agent performs prompt engineering itself — it generates the actual task prompt that gets handed off to the sub agent. You can watch this happen in the chat UI.
- **What the user sees:** Sub agent activity appears as a collapsible tool call in chat. Collapsed view shows the agent name and current tool (e.g., "Reading file..."). Expand it to see all tool calls, the generated prompt, and the returned result.
- **Custom agents as sub agents (Experimental):** Custom agents (`.agent.md` files) can be used as sub agents. A custom agent can define its own model, tools, and instructions — these override defaults when used as a sub agent.
- **`user-invocable: false`:** You can create "internal" agents that only exist to be invoked as sub agents — they won't appear in the agent dropdown for manual use.
- **`disable-model-invocation: true`:** Prevents an agent from being invoked as a sub agent by other agents. Only explicit user invocation allowed.
- **`agents` property:** A coordinator agent can restrict which custom agents are available to it as sub agents (whitelist by name, `*` for all, or `[]` for none).

## How It Works (Step-by-Step)

### Single Sub Agent Flow
1. User (or custom agent instructions) gives the main agent a complex task.
2. Main agent recognizes a portion that benefits from isolated context.
3. Main agent **writes a prompt** for the sub agent (this is the moment of agent-generated prompt engineering).
4. VS Code spawns the sub agent with that prompt and an isolated context window.
5. Sub agent works autonomously using its own tool calls (reads files, searches, etc.).
6. Sub agent returns a final result — either a text summary or writes to a file.
7. Main agent reads the result, integrates it into its own context, and continues.

### Parallel Sub Agent Flow
1. Main agent determines multiple independent subtasks can run simultaneously.
2. Main agent spawns all sub agents at once — each with their own prompt and isolated context.
3. All sub agents run in parallel.
4. Main agent waits for **all** sub agents to complete before proceeding.
5. Main agent synthesizes all results and continues.

### Workflow Loops
- After one sub agent completes, the main agent can spawn a fresh batch of parallel sub agents.
- After that batch completes, it can spawn another single sub agent.
- You can mix and match: one → parallel three → one → parallel two → one, etc.
- You can loop back and re-invoke a sub agent type (e.g., run a reviewer, apply fixes, run the reviewer again).

## Interesting Angles & Talking Points

- **The agent does prompt engineering.** The main agent literally writes a prompt for its sub agent. If you expand the sub agent tool call, you can read it. This is a meta-moment worth showing on screen — the AI writing instructions for another AI.
- **Context is the hidden constraint.** Most users never think about context windows until they break. Sub agents are fundamentally a context management tool, and framing them that way hits an exec-level nerve (token costs, reliability at scale).
- **The one-layer rule is a practical design decision.** No recursive nesting keeps orchestration predictable and auditable. Worth noting as an honest limitation — and it shapes how you design workflows.
- **You can build specialized sub agents.** An internal `security-reviewer` agent with access to security-specific MCP tools, a `planner` agent that only has read access — these are real architecture patterns for team AI workflows.
- **The built-in Plan agent already uses sub agents internally.** This is a concrete, real-world example of the pattern in the wild that users might have already seen without knowing it.

## Known Limitations & Gotchas

- **No interleaving parallel sub agents.** You can't react to sub agent A finishing while sub agent B is still running. You must wait for all parallel sub agents to finish before acting.
- **No recursive sub agents.** Sub agents cannot spawn their own sub agents. One level of delegation only.
- **`runSubagent` tool must be enabled.** Users who haven't touched Copilot tool settings might not have this on. Enabling it is a prerequisite to the whole feature.
- **Context isolation cuts both ways.** The sub agent doesn't know your conversation history — it only knows what the main agent puts in the prompt. If the main agent writes a weak prompt, the sub agent will produce a weak result.
- **Experimental features:** Custom agents as sub agents and the `agents` whitelist property are both marked experimental in the docs. Behavior may change.
- **Sub agents inherit the main agent's model/tools by default** unless a custom agent overrides them. Something to be aware of when designing specialized sub agents for cost optimization.

## Reference Material

- [Subagents in Visual Studio Code](https://code.visualstudio.com/docs/copilot/agents/subagents) — primary how-to doc, includes usage scenarios, invocation patterns, orchestration patterns
- [Agents concepts](https://code.visualstudio.com/docs/copilot/concepts/agents#_subagents) — conceptual background: context isolation, sync vs. parallel execution
- [Using agents in VS Code](https://code.visualstudio.com/docs/copilot/agents/overview) — broader agent ecosystem overview

## Suggestions

- **Open with the "AI writing a prompt for another AI" moment.** Show the expanded sub agent tool call — the generated prompt is right there in the UI. It's visually surprising and immediately communicates what makes this feature interesting.
- **Use your architecture diagrams as the backbone of the demo.** Let the diagram play while you narrate the flow, then cut to the actual VS Code UI showing it happening in real life. The visual match between diagram and reality is compelling.
- **Demo a parallel sub agent invocation live.** Show three sub agents spinning up at the same time in the chat view — the collapsible tool calls appearing simultaneously. It's a visual "wow" moment that's hard to convey in text.
- **Name the limitations clearly.** Calling out no-interleaving and no recursive nesting builds trust. It also plants the seed that this is a tool worth designing around, not something you just accidentally use.
- **Close on the coordinator/worker pattern.** The TDD example (Red → Green → Refactor agents) from the docs is a perfect, concrete example of what a production-grade sub agent workflow looks like. It connects the feature to something real teams would actually build.
- **Soft CTA angle:** "If you want help designing an AI workflow for your team" connects this directly to Bitovi's consulting pitch without being overt.

## Demo Scenario

**"You just vibe coded an entire app — now you need AI to make sure it will scale."**

The hook frames the context window problem naturally: AI generated thousands of lines of code fast, now you need a thorough review (security, performance, correctness, code quality) — but dumping all of that into a single Copilot chat risks hitting the context wall and producing shallow results. Sub agents are the solution.

**Demo flow:**
1. Show the large vibe-coded codebase — emphasize the volume of code
2. Main agent receives the task: "Review this codebase for production readiness"
3. Main agent spawns 3 parallel sub agents simultaneously (security review, performance review, code quality review) — each gets only the relevant files and a focused prompt
4. Show all three running at once in the chat UI (collapsible tool calls appearing in parallel)
5. Expand one to show the generated prompt — the main agent doing prompt engineering
6. All three complete; main agent synthesizes a prioritized findings report

**Secondary framing (one-liner):** "This is also exactly what happens at code review time on a team — PR just dropped with 800 lines changed."

## Production Decisions

- **Recording format:** Pre-recorded screen capture clips
- **Diagrams:** Basic subagent flow diagram (main agent → prompt generation → isolated context → result return)
- **Custom agents:** Brief mention only — explain the value (specialized tools, restricted access, dedicated model), tease a future video
- **Context management:** Seed a follow-up short at the end ("context windows deserve their own video — that's next")
- **FAQs:** Include as a rapid-fire section near the end
  - Do sub agents consume premium requests? → No
  - Does the sub agent have access to the main agent's conversation history or Copilot instructions? → No, only the prompt the main agent sends it
  - Are sub agents slower? → About the same speed (potentially a bit slower)
  - Does a sub agent inherit the main agent's tools and MCP? → Yes, and custom agent files can restrict them further
  - Do sub agents use copilot-instructions/skills automatically? → Unclear — sometimes yes, sometimes no (worth being honest about this)
- **CTA:** Both AI Readiness Check + Bitovi training offer

## Answered Questions

1. **What specific demo are you planning to show live?** Understanding the exact scenario (e.g., "main agent spawns a planner + reviewer sub agent in parallel for a real codebase task") will shape how the outline sequences the demo vs. the diagrams.

- We should brainstorm this 


2. **Which diagrams do you have?** You mentioned a main architecture diagram (spawn → prompt generation → isolated context → result return) and at least one parallel workflow diagram. Are there more? Knowing what's ready shapes how much we lean on visuals vs. live UI in the outline.

I have a diagram of a basic subagent flow that outlines the main agent 

3. **How deep do you want to go on custom agents as sub agents?** It's experimental and a whole sub-topic. Could be a one-sentence mention and "we'll go deeper in a future video," or could be a distinct section. Your call.

- Maybe we could just mention it and talk about why it's useful

4. **Are you doing a live screen recording demo or showing pre-recorded clips?** Changes the production approach significantly.

- Pre-recorded clips

5. **Context management video — is that a follow-up video in this same series?** If so, we can seed it at the end of this one ("context windows deserve their own video — that's coming next").

- Yeah it's a follow up short that will play off of this video

6. **Do the FAQs make the cut?** You mentioned you might skip them. If they're good, they could be a strong ending section or a companion short.

Here they are
- Do Subagents consume premium requests
    - no
- Does the subagent have access to the main agent's conversation history? Copilot instructions?
    - No, it only has access to the prompt that the main agent sends it.
- Are subagents slower than the main agent?
    - They run about as fast as the main agent (*potentially a bit slower)
- Does a subagent inherit the main agent's tools? MCP?
    - Yes and it’s possible to limit them further using custom agent files.
- Do subagents use copilot-instructions/skills automatically?
… it’s unclear, sometimes they do sometimes they don’t???


7. **What's the CTA for this video?** AI Readiness Check, Bitovi training offer, or something else?

- both