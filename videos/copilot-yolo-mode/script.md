This is what running Copilot in agent mode actually looks like.

I asked it to build a todo list app. Simple task. Watch what happens.

It wants to read some files — I have to approve that. Now it wants to run a terminal command — approve. It's asking if it can keep going — approve. Another tool call — approve. Another terminal command — approve.

I've done more clicking than coding.


By default, Copilot pauses and asks your permission for almost everything — running terminal commands, calling tools, even just continuing after a certain number of steps. There's a reason for that. But if you're working on something where you trust the task, all those interruptions kill any momentum you had.

Here's how to fix it.


There's a dropdown in the chat input most people have never touched. It's the permissions picker, and it has three levels.

Default Approvals is what you've been living with — every tool call gets a confirmation dialog. Bypass Approvals silences those dialogs and auto-approves tool calls. And then there's Autopilot — that's the one you want. Autopilot auto-approves everything and, crucially, it also handles the agent's own clarifying questions so it never stops and waits on you. It just runs until the task is done.

Same prompt. Same task. Watch the difference.

No approvals. No pauses. It built the whole thing.


Now, Autopilot resets every session — it's a per-conversation setting. If you want this behavior to stick, there are three settings worth knowing.

The first is chat.tools.global.autoApprove. That's the master switch for tool approvals — turning it on means Copilot never asks for permission to use a tool.

The second is chat.agent.maxRequests. This defaults to 25, which is why Copilot sometimes stops mid-task and asks "should I keep going?" Raise this to 50 or 100 and that prompt basically disappears on normal tasks.

The third one is less obvious. Copilot has a built-in deny list for terminal commands — and it blocks curl and wget by default. So if Copilot needs to run an install script, it'll stop and ask even with everything else turned on. You can customize that list under chat.tools.terminal.autoApprove and either add commands you're comfortable with or just clear the deny list entirely.


Now, if you want to go further — if you're working in a codebase where you genuinely don't want an AI agent touching your host machine at all — Docker has an answer for this.

It's called Docker Sandboxes, and it runs agents inside isolated microVMs. Each sandbox gets its own private Docker daemon. The agent has complete freedom inside the VM — it can install packages, run commands, spin up containers — but it cannot reach your host system. Your files sync in, the agent works, and nothing leaks out.

Docker actually calls this "YOLO mode by default" in their own documentation. That's not me editorializing — it's literally what they wrote.

Right now, Copilot support in Docker Sandboxes is still in development. Claude Code is production-ready today. Copilot is coming. But the architecture is already there, and this is clearly where things are heading for teams that want true agent autonomy without the exposure.


The interruptions in Copilot aren't a bug — they're a default posture. But defaults aren't destiny. Once you know which dial controls which prompt, you can tune this to match how you actually work.

At Bitovi, we help engineering teams set this up the right way — the settings, the workflows, and the guardrails that actually make sense for your team and your codebase. If you want your developers getting real value out of Copilot instead of fighting it, reach out. Link is in the description.
