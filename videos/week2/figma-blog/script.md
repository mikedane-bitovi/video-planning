# Figma Just Gave AI Agents Write Access to Your Canvas

## Description
This short is a visual companion to the Bitovi blog post about Figma's new `use_figma` capability. It shows the core workflow live: Claude Code connected to Figma, a prompt sent to the agent, the canvas updating in response, and the role skills play in making that output consistent with a team's design system.

## Script

Figma just redefined what's possible with AI Agents and design.

Before, agents could only read your designs. Now, they have "write" access.

Let's start with a totally blank canvas. I'm using Claude Code connected to the Figma MCP server. And I'll give it a simple prompt asking it to build me a todo list.

In order to do this, Claude will call the use_figma tool and provide it with instructions on what to build.

Give it a few seconds, and we have a fully polished design. And this isn't jsut a quick mockup, everything is labeled, organized into proper frames, and uses best practives. In other words, it's production ready.

But it gets better. You aren't just limited to generic designs. You can actually have the agent build things that integrate directly with your existing design system.

Let's say you have a library with specific buttons, inputs, and brand variables already set up. To get the agent to use them, you just need to give it a little more detail in the prompt.

You can do this directly from the chat, or, even better, by using an Agent Skill. A skill is just a set of instructions that tells the agent: "Hey, when you build this, use our 'Primary Brand' color variable and pull from our 'Core Components' library.". Figma actually provides official skills that are great to use.

When you provide that context, the agent stops guessing and starts building exactly the way your team does. It bridges the gap between a "cool AI demo" and your actual design workflow.

We took a deep dive into exactly how to set these skills up and tested it across several real-world scenarios. If you want to see the full breakdown and the results, check out the blog post linked below.