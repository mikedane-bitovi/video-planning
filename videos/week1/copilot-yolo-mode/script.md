I asked Copilot to build a todo app and had to click approve six times before it did anything. Read a file — approve. Run a command — approve. At this point there's more approving than coding. Not fun.

When you're in Agent mode, Copilot will prompt you to approve commands as they run. But often, there's an "Allow for this session" option that kills the interruptions — but it resets when you close VS Code. To make it permanent, you'll want to configure these four settings:

`chat.tools.global.autoApprove` — eliminates tool approval dialogs.

`chat.editing.autoAcceptDelay` — auto-accepts file edits after a delay, set it to 5000.

`chat.agent.maxRequests` — bump this from 25 to 100 so it stops asking "keep going?" mid-task.

`chat.tools.terminal.ignoreDefaultAutoApproveRules` — clears the built-in deny list that blocks curl and wget.

Those four settings and Copilot gets out of its own way.

There's also a new trend of using isolated Docker container to give Copilot even more autonomy, Docker is literllay calling it YOLO mode. Definitely something to look out for as this tech progresses.

We've been running this on real projects at Bitovi. If you want to train your team on the settings and guardrails that actually matter — link in the description.
