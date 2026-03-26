Something is shifting with AI agents — and GitHub Copilot is right at the front of it. Until recently, an agent could only do one thing at a time: you give it a task, it works through it sequentially, start to finish, all in one context window.

But now, agents can act as orchestrators. Instead of doing everything themselves, they can spin up sub agents — independent workers that each get their own task, their own context window, and run in parallel. The main agent delegates, the sub agents execute, and when they're done they report back. That's sub agents in GitHub Copilot.

This is one of the most significant upgrades to how Copilot works, a true paradigm shift — and most people haven't even touched it yet.

To use sub agents, just enable the runSubagent tool in your Copilot settings. Flip it on, and that's it — once it's enabled, the main agent decides when to use it completely on it's own. You can also explicitly request the use of subagents by referencing the tool directly in your prompt.

Here's what's happening under the hood. You give the main agent a task. Instead of doing everything itself, it spawns a subagent, writes a prompt for it, and hands off the work. That subagent gets a completely fresh context window — no conversation history, no prior noise — does its job, and returns the result. When it's done, it passes back a text result to the main agent either directly or by writing to a file.

The sub agent lives in its own bubble. What happens there stays there. Only the final output comes back.


Here's a simple case. We'll ask the main agent to write a README for our codebase. Without sub agents, it crawls through all your files, pulls in a ton of context, and by the time it starts writing, it's context window is bloated with content that's mostly not relevant to the README itself, just research noise.

With a sub agent, that research happens in isolation. The sub agent reads the files, builds up the findings, and hands a clean summary back to the main agent. The main agent never touches the noise — it just writes the README.

Taking a closer look, when you expand that subagent tool call, you can see the prompt the main agent wrote for the sub agent, that's not you prompting Copilot — that's Copilot prompting Copilot. Furthur down you can also see what the subagent returned back. Pretty cool.

Now the more powerful case is running multiple subagents at the same time — this is where Copilot becomes an orchestrator. Let's say we wanted to perform a full reivew of our codebase, checking it for security, performance and code quality.

Normally one agent would be trying to review the code on all those meterics at the same time — but with parallel agents, we just ask Copilot to spawn one subagent for each area, one for security, one for performance, and one for code quaitly.

Now each subagent can do it's own research, in it's own context window, and return back to the main agent only the relevant information for that one area. The main agent keeps it's own context window clean and focused, and we end up with a better review, faster. 

One consolidated code review. Three perspectives. None of them polluted by what the others found.

This really is a paradigm shift for AI agent workflows. Two things to keep in mind though. First, batches are all-or-nothing — if you launch three sub agents in parallel, the main agent waits for all three before it can move on. You can't interweave them and get too fancy.

Second, sub agents can't spawn their own sub agents. One level deep is the ceiling. It keeps the system predictable.

**[OUTRO]**
Sub agents are how you go from AI that helps you write code to AI that can manage real complexity.

Here at Bitovi, we've been deep in the weeds on this — researching different workflows and use cases for sub agents across real codebases. And we've found that when you use them well, they're genuinely a game changer for code quality and team throughput.

If you want to embrace this paradigm shift on your team, check out the link in the description. We'd love to talk with you about how to make the most of this — and we offer in-depth training on everything related to coding with AI, from sub agents and custom agents to prompt engineering and full workflow design. Link's below.

That's gonna do it for me, thanks for watching, and keep innovating. ß