# Outline: Run Copilot Without Approvals — The YOLO Mode Setup

## Overview
- **Format:** A1 — The Workflow Demo
- **Pillar:** AI Developer Enablement
- **Target Length:** ~2.5–3 minutes
- **Primary Audience:** Both — senior developers who want to move faster, and engineering leaders evaluating AI autonomy on their teams
- **Goal:** Viewer walks away knowing exactly which settings to change (and why) to stop babysitting Copilot, and learns that Docker Sandboxes is the "responsible YOLO" option for higher-stakes environments

---

## Scenes

### Scene 1 — The Hook: Show the Pain
- **Narration:** Open mid-task. "This is what running Copilot in agent mode actually looks like." Name the friction without editorializing — let the demo speak.
- **On Screen:** Screen recording of a Copilot agent task in progress — building the todo list app. Show the approval prompts stacking up: tool call confirmation, terminal command confirmation, "should I continue?" pause. 4–5 interruptions in quick succession.
- **Notes:** No intro, no setup. Start with the result — the annoyance. Keep this to 20–25 seconds max.

### Scene 2 — Name the Problem
- **Narration:** "By default, Copilot pauses and asks your permission for almost everything — running terminal commands, calling tools, even just continuing after a certain number of steps. There's a reason for that. But if you're working on something where you trust the task, all those interruptions kill your flow."
- **On Screen:** Green screen presenter. Could overlay a simple diagram showing the three main interruption types: tool approvals, terminal approvals, continuation prompts.
- **Notes:** Keep it brief — 15–20 seconds. Don't over-explain the "why it exists" angle or it gets defensive.

### Scene 3 — The Quick Fix: Autopilot Mode
- **Narration:** "The fastest way to fix this is a single dropdown in the chat input. Most people have never clicked it." Show where it is. Explain the three levels: Default Approvals, Bypass Approvals, and Autopilot — and what each one actually does. Autopilot is the key one: it auto-approves tool calls AND auto-answers the agent's own clarifying questions so it keeps running.
- **On Screen:** Screen recording zoomed into the chat input area. Click the permissions dropdown, show the three options, select Autopilot. Then re-run the same todo list app task from Scene 1 — this time it runs straight through with no interruptions.
- **Notes:** This is the "reveal" beat. The contrast with Scene 1 should be visually obvious — one version pauses constantly, this one just runs. ~35–40 seconds.

### Scene 4 — The Settings: Persistent YOLO
- **Narration:** "Autopilot resets every session. If you want this to be the default, there are a few settings worth knowing." Walk through: (1) `chat.tools.global.autoApprove` — the master switch for tool approvals. (2) `chat.agent.maxRequests` — raise this from 25 to avoid the "should I keep going?" prompt on longer tasks. (3) The terminal deny list — mention that `curl` and `wget` are blocked by default, and show `chat.tools.terminal.autoApprove` to customize it. Don't dwell — name them, show where they are, move on.
- **On Screen:** Screen recording of the VS Code Settings UI (not JSON). Navigate to each setting by name, show the current default, and change it. Keep it scannable — viewer doesn't need to read every word, they need to know these exist.
- **Notes:** ~40–45 seconds. This is the "here's how to make it stick" beat. Don't turn it into a settings lecture.

### Scene 5 — The Responsible YOLO: Docker Sandboxes
- **Narration:** "Now, if you're working on something where you genuinely don't want Copilot touching your host machine — running install scripts, modifying files outside the project — there's a better answer than just raising your risk tolerance." Introduce Docker Sandboxes. Explain the core idea: microVM, private Docker daemon, agent runs inside it and can't reach your host. Docker literally calls this "YOLO mode by default." One command to spin it up. Mention Copilot support is in development — Claude Code is production-ready today, Copilot is coming.
- **On Screen:** Green screen presenter over a simple architecture diagram showing host vs. microVM boundary. Then briefly show the `docker sandbox run` command in a terminal. Optional: show the Docker Desktop docs page with "YOLO mode by default" visible.
- **Notes:** ~35–40 seconds. Frame this as "the next level" — it's for teams or higher-stakes tasks, not just personal preference. The "YOLO mode by default" quote from Docker's own docs is sticky and quotable.

### Scene 6 — Consequence + CTA
- **Narration:** "The interruptions in Copilot aren't a bug — they're a default posture. But defaults aren't destiny. Once you know which dial controls which prompt, you can tune this to match how you actually work. At Bitovi, we help engineering teams set this up the right way — the settings, the workflows, the guardrails that make sense for your team specifically. If you want your developers actually getting value out of Copilot instead of fighting it, reach out. Link in the description."
- **On Screen:** Green screen presenter. End card with direct CTA: "Talk to Bitovi → [link]" prominently displayed.
- **Notes:** ~25 seconds. Explicit — this is the whole point of the video.

---

## B-Roll & Asset Checklist
- [ ] Screen recording: Copilot agent task with all approval prompts visible (Scene 1)
- [ ] Screen recording: Same task running in Autopilot mode — no interruptions (Scene 3)
- [ ] Screen recording: Permissions dropdown zoomed in — show all three levels (Scene 3)
- [ ] Screen recording: VS Code settings.json with the three key settings (Scene 4)
- [ ] Diagram: Three interruption types (tool approvals, terminal approvals, continuation prompts) — simple, clean (Scene 2)
- [ ] Diagram: Host vs. microVM boundary showing agent isolation (Scene 5)
- [ ] Screen recording or screenshot: `docker sandbox run claude` command + Docker docs "YOLO mode by default" line (Scene 5)
- [ ] End card graphic with CTA link

---

## Suggestions
- The Scene 1 → Scene 3 contrast is the heart of the video. Make sure both recordings use the exact same task so the comparison is apples-to-apples.
- "Defaults aren't destiny" in Scene 6 is a good closing line — it reframes the whole video as empowerment, not workaround.
- Consider a text overlay in Scene 3 that labels the Autopilot option as it's selected — viewers watching on mobile or at speed will miss the dropdown text otherwise.
- The Docker "YOLO mode by default" quote is gold — put it on screen as a pull quote, don't just say it.

## Open Questions

All resolved.
