# Research: Copilot YOLO Mode — Running Copilot Agent Without Approval Interruptions

## Topic Summary

GitHub Copilot in VS Code agent mode is powerful, but by default it constantly pauses to ask for your approval — before running terminal commands, calling tools, fetching web content, or continuing after a long run. For developers who trust their workflow and want Copilot to just execute autonomously, there's a full suite of settings and a new permission system that can eliminate these interruptions. This video walks through the complete "YOLO mode" setup: what each guardrail does, what each unlock unlocks, and the one or two places where you probably still want a speed bump.

## Key Facts & Concepts

- **Agent mode** (VS Code 1.99+, now GA in Stable) is the primary context — this is where all the approval prompts live
- **Three permission levels** can be selected from a dropdown directly in the chat input area:
  - **Default Approvals**: uses your configured settings; tools that require approval show a confirmation dialog
  - **Bypass Approvals**: auto-approves all tool calls without any confirmation dialogs
  - **Autopilot (Preview)**: auto-approves all tool calls, auto-responds to questions, and the agent continues working autonomously until the task is complete
- **`chat.tools.global.autoApprove`** — setting-level equivalent of "approve everything." Warning in the docs: "this setting disables critical security protections." Default: `false`
- **`chat.autopilot.enabled`** — controls whether the Autopilot permission level appears in the picker. Default: `true`
- **`chat.tools.terminal.enableAutoApprove`** — master switch for terminal command auto-approval. Default: `true`
- **`chat.tools.terminal.autoApprove`** — granular control over which terminal commands are auto-approved vs. blocked. Comes with a **default deny list**: `rm`, `rmdir`, `del`, `kill`, `curl`, `wget`, `eval`, `chmod`, `chown`, and `Remove-Item`. Supports regex patterns.
- **`chat.tools.terminal.ignoreDefaultAutoApproveRules`** — ignore the default deny list. Default: `false`
- **`chat.agent.maxRequests`** — max number of requests Copilot makes before stopping to ask "should I keep going?" Default: `25`. Raise this to reduce "continue?" interruptions.
- **`chat.editing.autoAcceptDelay`** — delay (in ms) after which suggested edits are automatically accepted. Set to a non-zero value to auto-accept. Default: `0` (disabled)
- **`chat.tools.edits.autoApprove`** — configure which files require approval before edits are applied. Uses glob patterns. Default: `{}` (everything requires approval)
- **`github.copilot.chat.summarizeAgentConversationHistory.enabled`** — auto-summarizes conversation history when the context window fills up, preventing the session from stalling. Default: `true`
- **`chat.tools.terminal.sandbox.enabled`** (Experimental, macOS/Linux) — sandboxes terminal commands with restricted filesystem/network access. **When enabled, commands are auto-approved** because the sandbox handles safety. This is an interesting middle path.
- **`github.copilot.chat.claudeAgent.allowDangerouslySkipPermissions`** — bypass all permission checks specifically for the Claude agent. Docs say: "Only enable this in isolated sandbox environments."
- **Tool approval memory** (added in v1.99) — when Copilot asks for approval, you can choose to remember that approval at the **session**, **workspace**, or **application** level. Not available for terminal commands yet (coming).
- **`chat.notifyWindowOnConfirmation`** — shows an OS notification when user input is needed. Useful if you step away and want to know Copilot has paused.

## How It Works (Step-by-Step)

**The fast path: Autopilot mode per session**
1. Open Chat view (⌃⌘I)
2. Select Agent mode from the mode picker
3. Click the **permissions dropdown** in the chat input area
4. Select **Autopilot** — Copilot will now auto-approve all tools, auto-answer its own clarifying questions, and continue working until it's done

**The persistent path: Settings-based YOLO**
1. Set `chat.tools.global.autoApprove: true` — eliminates all tool approval dialogs
2. Set `chat.tools.terminal.enableAutoApprove: true` (already default)
3. Optionally clear/ignore the default terminal deny list via `chat.tools.terminal.ignoreDefaultAutoApproveRules: true` (nuclear option)
4. Raise `chat.agent.maxRequests` to something like 50–100 to reduce "keep going?" prompts
5. Set `chat.editing.autoAcceptDelay` to a value like 5000 (5 seconds) to auto-accept file edits
6. Enable `github.copilot.chat.summarizeAgentConversationHistory.enabled` (already default `true`)

**The sandboxed YOLO path** (macOS/Linux)
1. Enable `chat.tools.terminal.sandbox.enabled`
2. Configure allowed/denied paths in `chat.tools.terminal.sandbox.macFileSystem`
3. Commands are auto-approved but restricted — good for untrusted repos or exploratory sessions

## Interesting Angles & Talking Points

- **The friction is intentional** — VS Code's default behavior is designed to keep you in control. Each approval prompt is a "are you sure?" moment. YOLO mode is about opting out of those moments deliberately.
- **The permission dropdown is new and underutilized** — most people don't know it exists. Autopilot mode was added relatively recently and removes virtually all friction in a single click, per session.
- **The default terminal deny list is surprisingly opinionated** — `curl` and `wget` are blocked by default. This means even a simple "run this install script" will pause unless you tweak it.
- **Raising `maxRequests`** is often overlooked — most users hit the "should I continue?" prompt and think Copilot is broken or done, not realizing they can simply raise this limit.
- **The sandbox option is the "responsible YOLO"** — it lets Copilot run freely while still protecting the rest of your system. Worth highlighting as the middle path.
- **There's no single "YOLO switch"** — you need to know which setting controls which type of interruption. That's the value of this video: mapping each interruption to its cure.
- **Autopilot vs. Bypass Approvals** are subtly different — Bypass just skips confirmation dialogs; Autopilot also handles the agent continuing on its own when it'd normally stop and ask a question.

## Known Limitations & Gotchas

- **`chat.tools.global.autoApprove`** is flagged in the docs as disabling "critical security protections" — worth acknowledging honestly in the video
- **Terminal command approval memory** for the per-command level isn't fully implemented yet (mentioned as "we plan to develop this") — so session-level Autopilot is the best available option for terminal freedom right now
- **`chat.agent.maxRequests`** doesn't eliminate the "continue?" prompt entirely at very high task counts — it just pushes the limit further
- **Autopilot mode resets per session** — it's a per-conversation setting, not a persistent one (unless you flip the underlying global settings)
- **MCP tool approvals** are covered by `chat.tools.global.autoApprove` but individual MCP server tools can also be toggled via the Select Tools button
- **`chat.editing.autoAcceptDelay`** auto-accepts *all* edits after the delay — there's no "only auto-accept if I'm idle" logic, so you could miss changes

## Docker Sandboxes — The Hardware-Level YOLO

Docker Sandboxes (Experimental, Docker Desktop 4.58+) is a purpose-built solution for this exact problem. Docker literally uses the phrase **"YOLO mode by default"** in their own documentation.

**What it does:**
- Runs AI coding agents inside isolated **microVMs** — each with their own private Docker daemon
- Agents have full autonomy (no approval prompts) but can't touch your host system
- The agent runs inside the VM; it can't access your host Docker daemon, other containers, or files outside the workspace
- Your workspace directory syncs between host and VM at the same path, so file paths in error messages still match
- Network access is configurable via policy rules

**How to use it:**
```
cd ~/my-project
docker sandbox run claude
```

**Supported agents:**
- Claude Code (production-ready)
- Copilot — **listed as "in development"** as of March 2026
- Codex, Gemini, OpenCode, Kiro, Docker Agent — all in development

**Requirements:** macOS or Windows for microVM sandboxes; Linux gets legacy container-based sandboxes (Docker Desktop 4.57+)

**The key insight for the video:** This is the "responsible YOLO" ladder rung above VS Code's built-in sandbox setting. VS Code's `chat.tools.terminal.sandbox.enabled` restricts what commands can do at the OS level. Docker Sandboxes goes further — it isolates the entire agent in a VM so even if it does something destructive, it can't touch your host. For teams evaluating AI agents on production-adjacent code, this is the enterprise-grade answer.

**Caveat:** Copilot support in Docker Sandboxes is still in development. This is worth mentioning — it's coming, and the architecture is ready, but it's not fully wired up yet.

## Reference Material

- VS Code v1.99 Release Notes: https://code.visualstudio.com/updates/v1_99 (Agent mode GA, tool approvals, auto-approve settings)
- GitHub Copilot Settings Reference: https://code.visualstudio.com/docs/copilot/copilot-settings (complete settings list)
- Chat Agent Mode docs: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
- Chat overview (Permission levels section): https://code.visualstudio.com/docs/copilot/chat/copilot-chat
- Docker Sandboxes: https://docs.docker.com/ai/sandboxes/ (microVM isolation for AI agents, "YOLO mode by default")

## Suggestions

- **Lead with the Autopilot dropdown** — most viewers will have never seen it. That's your hook: "there's a switch in the UI most people don't know about."
- **Demo the friction first, then remove it** — start with a real agent task, show all the "Allow?" prompts piling up, then switch to Autopilot and re-run the same task. The contrast is immediate.
- **Use the terminal deny list as a memorable detail** — "VS Code won't let Copilot run curl by default" is a surprising, specific fact that sticks.
- **Frame Docker Sandboxes as the advanced tier** — "if you want true YOLO, Docker literally calls it that." Position it as the next step for teams, not just individuals.
- **The YOLO spectrum** is a compelling narrative arc: Default (everything asks) → Bypass Approvals (tool calls silent) → Autopilot (fully autonomous session) → VS Code sandbox (auto-approve + OS restrictions) → Docker Sandbox (microVM isolation, nothing can reach your host). Each level trades control for speed.
- **Thumbnail idea**: the word "YOLO" over a VS Code + Docker screenshot, or a "permission denied" to "permission granted" visual.
- **Good CTA target**: an AI Readiness Check or the Bitovi Copilot training content — this video is aimed at people actively using Copilot and ready to go deeper.

## Open Questions

1. **Do you want to show the UI (Autopilot dropdown) as the primary demo, the settings JSON as the primary demo, or both?** The UI approach is more visual and accessible; the settings approach is more "power user."
2. **How far do you want to go with the terminal deny list?** Showing `ignoreDefaultAutoApproveRules: true` is edgy and memorable, but might make some viewers nervous. Intentional?
3. **Should the demo use a real project task** (e.g., "scaffold a Node app and install dependencies") to show all the interruption types in one sequence, or keep it abstract?
4. **How prominently should Docker Sandboxes feature?** Copilot support is still "in development" there — worth a mention as "this is the direction things are heading" or skip until it's fully supported?
5. **Target audience split:** Is this primarily for the individual developer who wants to move faster, or are you framing it for engineering leaders thinking about team-wide policy?
6. **Are there internal Bitovi workflows or client stories** where going hands-off with Copilot produced a notable result worth referencing?
