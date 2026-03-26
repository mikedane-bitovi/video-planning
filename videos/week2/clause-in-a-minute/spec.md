# Claude Code in a Minute

## Meta
- **Estimated length:** 60-90 seconds
- **CTA:** Bitovi can help your team adopt workflows like this without the usual trial and error.

---

## Step 1: Research [x]
Raw notes, sources, key facts, and context on the topic.
Everything in the script should trace back here.

### Positioning
- Claude Code is an agentic coding tool, not just a chat window. Official docs describe it as able to read a codebase, edit files, run commands, and integrate with development tools.
- It works across terminal, IDE, desktop, browser, CI, and automation surfaces, but the CLI/terminal experience is the core identity.

### Selected five for this short
- **Large context / long-horizon tasks:** Claude Code is built for codebase-level reasoning and multi-step work. In practice, that means you can pull in a large amount of repo context, ask it to understand the architecture, trace flows across files, and stay on task long enough to actually complete meaningful work.
- **Images and screenshots:** Claude Code can analyze screenshots, mockups, diagrams, and other images. The docs explicitly position this for things like understanding UI designs, reasoning about diagrams, and using screenshots as debugging or implementation context.
- **MCP integrations:** Claude Code can connect to external tools and data sources through MCP. Official examples include GitHub, Jira/Confluence via Atlassian, Figma, Slack, Sentry, and databases, which means Claude can work with context that lives outside the repo instead of making you copy-paste everything in.
- **Shell commands and operating system access:** Claude Code can run shell commands, inspect the filesystem, execute tests, and generally operate through the terminal as part of getting a task done. That makes it much closer to an autonomous coding agent than a text-only assistant.
- **Subagents, agent teams, and parallel work:** Claude Code can delegate work to specialized subagents, and it also supports agent teams plus worktree-based isolation so multiple tasks can run in parallel without stomping on each other. This is more than “one agent doing one thing” because it supports concurrent, isolated work.

### Bonus capability
- **Generate diagrams and visual artifacts:** Claude Code can help produce visual outputs as part of workflows, whether that is generating diagram markup, creating supporting files, or orchestrating scripts that output HTML or other visual artifacts. This is better framed as a workflow capability than a single built-in “diagram button.”

### Notes for the short
- The short should not imply every feature will be shown live. This concept works better as a rapid-fire capability roundup.
- Best flow is now a “five things you might not know” structure, where each item gets a concrete explanation instead of just a label.
- The order for this version should be: large context first, then images, then MCP, then shell commands, then subagents/agent teams/parallel work.
- Subagents and parallel work should be distinguished clearly:
	- A **subagent** is Claude delegating a task to a specialized helper.
	- **Agent teams / worktree parallelism** is Claude spinning up multiple isolated workers so work can happen in parallel.
- The bonus button at the end can be diagram generation or visual output, but it should be framed carefully so we do not overstate it as a single native feature.

### Suggested framing
- Opening contrast: “Most people are still using Claude Code for about ten percent of what it can actually do.”
- Payoff: “Here are five things Claude Code can do that most devs still haven’t realized.”
- Tone should feel like a technical peer giving a fast tour, not a tutorial.

### Sources
- Official overview: https://code.claude.com/docs/en/overview
- Common workflows: https://code.claude.com/docs/en/common-workflows
- MCP docs: https://code.claude.com/docs/en/mcp
- Memory docs: https://code.claude.com/docs/en/memory
- Skills / slash commands docs: https://code.claude.com/docs/en/slash-commands
- Hooks docs: https://code.claude.com/docs/en/hooks
- GitHub Actions docs: https://code.claude.com/docs/en/github-actions

---

## Step 2: Define the World [ ]
- **Scenario:**
- **Friction:**
- **Resolution:**
- **Viewer takeaway:**

---

## Step 3: Mission Brief [ ]
Major sections and narrative arc — where does the viewer start and where do they end?

**Mission brief (1-2 sentences):**

---

## Step 4: Value & CTA [ ]
- **Insight bridge:**
- **Secret sauce:**
- **CTA (specific offer):**

---

## Step 5: Storyboard Notes [ ]
Scene-by-scene breakdown with rough timing, b-roll plans, and visual element notes.
(Full storyboard lives in Google Slides — these are the notes to build from.)

---

## Step 6: Script [x]

Most people are still using Claude Code for like ten percent of what it can actually do. Here are five things Claude Code can do that most developers still do not realize.

First, the context window is big enough that you can have Claude pull in an entire codebase worth of context, understand how the system fits together, and stay on a task long enough to do real multi-step work.

Second, it can analyze screenshots, mockups, and diagrams. So if the context is visual, not just code, Claude can still reason about it.

Third, with MCP, Claude can read data from third-party tools too. So now it is not just your codebase and your screenshots, it is also GitHub, Jira, Figma, Slack, Sentry, databases, whatever you connect.

Fourth, it can run shell commands and work directly with your operating system and file system. It can inspect files, run tests, execute commands, and actually do the work, not just talk about it.

Fifth, it can use subagents, agent teams, and parallel work. So Claude is not limited to one linear thread, it can delegate specialized tasks and spin up isolated workers in parallel.

And as a bonus, it can even help generate fun diagrams and visual artifacts.

That is why Claude Code is interesting. It is not just a chatbot. It can read more context, use more tools, and do more of the job.
