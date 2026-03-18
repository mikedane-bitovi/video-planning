You just used AI to build an entire app. Now comes the part nobody talks about — how do you actually know if this code is any good?

You could dump all of it into a single Copilot chat and ask it to review everything. But if there's enough code, you're going to hit the context wall. And even if you don't — one agent trying to review security, performance, AND code quality all at once? That's not a real review. That's a skim.

So instead — this. Three separate AI agents, running at the same time, each with a different job. And that's what we're covering today — sub agents in GitHub Copilot.


What you just saw is the main agent splitting the work. Instead of trying to hold your entire codebase in one context window, it spawns independent sub agents — one for security, one for performance, one for code quality — each working in its own isolated context, doing its job, and reporting back.

Think of it like spinning up a team. The main agent is the coordinator. The sub agents are the specialists.

This coordination pattern, where an agent orchestrates a team of subagents is quickly becoming popular due to it's flexibility and power. In this video, we'll take a look at how it works. 


Before we go further — to use sub agents, you need to make sure the runSubagent tool is enabled in your Copilot settings. Find it, flip it on. That's literally it. Once it's enabled, the main agent decides when to use it — you don't have to do anything special in your prompts.


Now let's look at the simplest case. Say our todo app doesn't have a README yet.

Here's the naive approach: just ask the main agent to write one. It'll go read through all your files, try to figure out what the app does, pull in a bunch of context it may or may not need — and by the time it actually starts writing, your conversation is bloated with all that intermediate work. Half of it probably wasn't even useful.

A sub agent handles this better. The sub agent goes off and does all the messy research — crawling through files, figuring out what actually belongs in a README — and hands a clean summary back to the main agent. The main agent never has to look at any of that noise. It just gets the research, and it writes the README. Each one focused on its job.

You can invoke a sub agent explicitly by typing hash runSubagent in the chat — or you can just describe what you want and the agent will figure out that a sub agent makes sense. Either way, watch what happens when you expand that tool call.

Right there — that's the prompt the main agent wrote for the sub agent. The agent just did prompt engineering. It reasoned about the task, wrote instructions for another AI, and handed it off. That's not you prompting Copilot. That's Copilot prompting Copilot.


So here's what's actually happening under the hood. The main agent generates a prompt and spins up a sub agent with a completely clean context window — no conversation history, no prior context, just that task. The sub agent does its own work: reads files, runs searches, builds up its findings. When it's done, it either returns a text summary or writes results to a file. The main agent picks that up, integrates it, and keeps going.

The key thing: that sub agent lives and dies in its own bubble. What happens in there stays in there — only the final result comes back.

This is especially useful for writing documentation, because the subagent can do all the research and the main agent can focus on creating the best file possible. 


Now let's go back to where we started. Three sub agents running at the same time — this is parallel execution. In this case, we're having the agent do a full review of our code accross security, performance, AND code quality. 

The main agent launches all three at once, each one does its focused review, and then — this is important — the main agent waits for all of them to finish before it does anything with the results.

Not one. Not two. All three. Then everything flows back in, and the agent synthesizes a verdict.

One consolidated code review, from three different perspectives, none of them polluted by what the others found.


Once you've got the basic mechanics down, you can start designing real workflows. And there's a lot of flexibility here.

You can run a single sub agent, then spawn a batch in parallel, then another single one after. You can build loops — run a reviewer sub agent, apply the fixes, run it again. Go as many rounds as you need.

But there are two things you can't do.

Say you launch two sub agents in parallel. One finishes early. You might think — great, let's kick off the next task while the other one is still running. You can't. It's all-or-nothing per batch. All launched agents have to finish before the main agent can move on.

And sub agents can't spawn their own sub agents. One level deep, that's the ceiling. It keeps the whole system predictable — you always know who's running what.

If you're thinking about rolling this out on your team — designing workflows, figuring out what guardrails make sense — that's exactly the kind of thing we help teams work through. Link in the description.


One more layer worth knowing about: custom agents as sub agents. You can define agents specifically built for this role — a security reviewer wired up to the right MCP tools, a planner that's restricted to read-only access, each with its own model and instructions. When your main agent delegates, it hands off to one of these specialists instead of a generic instance. We'll go deep on this in a future video — it's a whole topic on its own.


Quick round of the questions people always ask.

Do sub agents use premium requests? No, the main agent and any subagents it spawns are covered under the one request.

Do they see your conversation history? No. They only know what the main agent puts in that prompt.

Are they slower? About the same speed, maybe a tiny bit slower.

Do they inherit your tools and MCP servers? Yes — and if you want to restrict that, custom agent files let you lock it down further.

Do they follow your copilot-instructions automatically? According to the docs, sub agents don't inherit the main agent's instructions — they only get the task prompt. Whether your workspace copilot-instructions.md still gets picked up is actually not addressed in the docs, so that one's worth testing.


Sub agents are how you go from AI that helps you write code to AI that can actually manage complexity at scale. Multi-step workflows, parallel analysis, loops — all of it coordinated by a single main agent.

The teams that figure this out first are going to ship faster, review code better, and do more with fewer people. The teams that don't are going to wonder why their AI setup still feels like a fancy autocomplete.

If you want to know where your team actually stands — what's working, what's missing, what you should build next — we run an AI Readiness Assessment here at Bitovi specifically for engineering teams. It's not a sales call. It's a real look at your setup and a concrete plan for what to do about it. Link in the description.

And if you want to actually train your team on this stuff — sub agents, custom agents, prompt engineering, the whole workflow — we do that too. Also in the description.

And if you're wondering what's really going on with context windows — why they matter, how they break, how to design around them — that's the next video.
