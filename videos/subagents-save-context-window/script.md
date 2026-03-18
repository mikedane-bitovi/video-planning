The longer you chat with an AI agent, the dumber it gets. Here's why — and how to fix it.

Every AI agent has a context window — think of it as its working memory. Everything in the conversation lives there: your messages, its responses, any files it reads, any tools it runs. The bigger it gets, the more the agent has to wade through to figure out what actually matters. And it has a hard limit.

I've got a log file here — a thousand lines of events from my app. I'm going to ask the agent to read through it and flag anything suspicious. Watch what happens to the context window.

It found a few suspicious logs. But now all thousand lines of that log file are permanently loaded into that working memory. Every follow-up question I ask, the agent is wading through all of that noise trying to figure out what's actually relevant. And once the window hits its limit, the AI starts compacting — automatically summarizing the conversation down to make room — and that's where you really start to lose it. Detail disappears, the agent drifts, and you're left wondering why it stopped being useful.

Now watch this instead. Same file, same prompt — but this time the agent uses a sub agent. A sub agent is a completely separate AI instance that spawns off the main agent with its own isolated context window — its own bubble. It goes off, reads the entire file, does all the work, and when it's done that bubble disappears. The main conversation only gets back the result — just the findings, nothing else.

Clean context. No compaction. The agent knows exactly what we're talking about.

This works for any situation where you need to bring in a lot of information to extract a little signal — large files, documentation, codebases. If you want your team actually doing this well, we train engineers on exactly these kinds of context strategies at Bitovi. Link in the description.
