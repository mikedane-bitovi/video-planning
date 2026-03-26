# One-Shot a Feature with Claude Code, Jira & Figma

## Description
This video shows a full Claude Code workflow where a single Jira ticket, paired with embedded Figma links and Bitovi's `generate-feature` prompt, becomes a working feature in one pass. It covers the end-to-end setup: Jira as the source of truth, Figma as the design context, MCP as the bridge, and Claude Code as the agent that ties it all together.

## Script

I just one-shotted a Jira ticket with Claude Code, and not only did it work, it also matched my Figma designs exactly.

To show this off, imagine I'm a developer working on this todo list app. And let's say my product manager specs out a new feature in a Jira ticket. They want to add priority labels for each todo item, Low, Medium and High.

The Jira ticket links out to Figma designs that show exactly what this should look like, including some options for filtering and sorting.

Now, normally I might sit down and build this myself, but let me show you how I can get Claude Code to read the Jira ticket, inspect the designs, and write the code in just a couple minutes.

All I need to do is open Claude Code, tell it the ID of the Jira ticket, and paste in the link to one of Bitovi's "magic prompts" (this is the secret sauce, more on these later)

Let's watch as Claude starts working.

First thing it does is call out to Jira to get the ticket info. It reads the ticket and figures out what we want to build.

Then, in parses out the Figma links and looks at each design to figure out what the new feature should look like.

Agents like Claude are surprisingly good at understanding designs, and Figma provides a really nice interface for this which we'll dive into a bit later.

Once it understands the requirements and designs, it's ready to start implementing.

When it's done, we can test out the new feature and, if nescessary, ask Claude to fix any issues.

So all we had to do was tell it the ID of the Jira ticket and paste in a link to a prompt and within a few minutes we have a working feature!

Let me also show you something cool. This app has a Storybook component library, and look — Claude added the new priority badge component right in there too. It didn't just write the feature, it fit it into how the codebase was already organized.

So that's the basic workflow, now let's dive into exactly how it works.

We'll start with the Jira ticket, because how you write it directly affects what Claude produces.

Here's the ticket I used. The requirements are written in Gherkin format — using Given, When, Then language.

It's a structured way to describe behavior, and AI agents respond really well to it because it's explicit and unambiguous.

The other important thing: the Figma design links are embedded right in the ticket description. In Figma, you can right-click any frame and copy a direct link to just that frame. The more you can point Claude at exactly the right part of the design, the better the output.

Keep in mind that the better you can outline what you want to build in the ticket and designs, the better Claude will do at implementing it. This sounds simple, but it's often the biggest problem when working with agents, not being speciic enough about what you want.

So Jira is looking good, now over in Figma — there's one thing you can't skip. Down at the bottom of the screen there's a toggle for Dev Mode. This is what allows developer tools like Claude to access information about your designs. In order for Claude to read your designs you need to have an account with access to dev mode, if you can toggle this on, you're good to go.

Enabling Dev Mode also turns on an MCP server for Figma — MCP is at the heart of this entire workflow so let's take a second to talk about it.

MCP or Model Context Protocol, is how AI Agents like Claude can easily talk to external services like Jira or Figma. Using MCP, Claude can ask Jira for a specific ticket, or Figma for information about what a design looks like.

Each 3rd party service has a set of "tools" that Claude can use to accomplish it's task. And setting up an MCP connection is easier than you might think.

There are actually a couple options for configuring MCP in Claude Code, but the most straightforward is to create an `.mcp.json` file at the root of your project — here's what it looks like with both the Figma and Atlassian MCP servers configured. We just need to include the URL to the mcp server along with a descriptive name.

You can also add servers with a terminal command, but the `.mcp.json` approach is better for teams because it lives in the repo and everyone gets the same config automatically.

Once that file is there, open Claude Code, type `/mcp`, and it walks you through authenticating with each service — Atlassian for Jira, Figma for designs. It opens in the browser, same as a normal login. After that, Claude has access to both and stays connected.

Now that Claude is connected to Jira and Figma, the final piece is that "magic prompt" we discussed earlier.

Bitovi has an entire repository of battle tested prompts on GitHub that you can just paste into the chat window for Claude to read.

In this case there's a prompt in the `writing-code` folder called `generate-feature`. This prompt walks the agent through this workflow:

Reading the Jira ticket, inspecting the designs, understanding the codebase and finally implementing the feature pixel perfect, with minimal changes.

This same basic workflow can used in a variety of circumstances. The central idea is that you connect Claude to external services via MCP, and then write a detailed orchestration prompt to handle the implementation (or whatever it is you want it to do).

All of your favorite apps are investing in MCP. Slack, Canva, Notion, Stripe, and databases like Postgres and Supabase. You can imagine the possibilites here.

And that's exactly the research we've been doing at Bitovi. We work with AI agents across real teams, figure out what actually works, and put it into prompts anyone can use.

If you want to bring this into your own team's workflow — the setup, the prompt engineering, structuring your Jira tickets so AI can actually use them — that's exactly what we help with. Link in the description.

That's gonna do it for me, thanks for watching, and keep innovating!