# How to Get Claude Code to See Your Figma Designs

## Description
This video is a focused walkthrough of the Figma MCP setup inside Claude Code. It explains how Claude can inspect Figma files, read tokens and metadata, generate screenshots, and even write back to the canvas, all without turning this into a full dev workflow demo.

## Script

Did you know Claude can look at your Figma designs and use them to build pixel-perfect UIs?

Actually, it goes much deeper than just "looking." Using Figma's Model Context Protocol tools, Claude can inspect the underlying structure of your files, learn your design system, and - with the new `use_figma` tool - it can even build designs directly on the canvas for you.

Let's take a look.

On the left, I have a component library for an app I'm building. On the right, Claude Code is ready to roll. Let's grab a link to one of these components, and ask Claude to inspect it.

Now with the right setup, Claude doesn't just see a picture; it gets the full design context - detailed CSS, spacing variables, metadata or even a screenshot.

I can also ask Claude to create a new design directly on the Figma canvas, and within seconds, I have a new component that fully integrates into my existing design system.

All of this is possible with MCP. For those new to the term, MCP is the standard for how AI agents communicate with apps like Figma, Slack, or Jira. It's essentially a bridge that gives Claude a "toolbox" to interact with your Figma designs. And it only take a few seconds to set up.

The first thing you'll need is a Figma plan with Dev Mode access. Look at the bottom of your Figma window for the mode switcher, if you can toggle Dev Mode on you're good to go, otherwise you'll need to upgrade your plan.

Once dev mode is enabled, in Claude Code, run the one-line config command to add the Figma MCP server to your global config. This makes Figma available across all your projects.

If you're working with a team, you can also define this in a `.mcp.json` file within a code repo, which helps if you're usign this to devleop an app.

One more thing to note. There are actually two ways to run Figma's MCP, either directly from the Figma desktop app, or using a remote url hosted by Figma. In this video we'll use the Figma hosted version as it tends to have the latest tools enabled first.

Once configured, just type `/mcp` in Claude Code. It'll prompt you to log into Figma via your browser, and just like that, you're connected.

Now that we're linked, let's look at the "toolbox" Claude is actually using to interact with Figma. Understanding these helps you prompt more effectively.

First we have the read tools:

get_design_context is the workhorse. Give it a link, and it returns the design as clean code. It defaults to React and Tailwind, but you can ask for any framework.

get_variable_defs pulls your design tokens-colors, spacing, and typography-ensuring the code Claude writes actually matches your theme.

search_design_system let's Claude look through your designs to learn more about what's available.

Then there's the write tools:

use_figma is the newest and it's a game-changer. Claude can create, edit, or delete layers directly on your canvas. Imagine describing a new layout and watching Claude generate the first draft in Figma while you watch.

And if you need a fresh start, create_new_file can spin up a new Figma Design or FigJam file instantly.

The best part? You rarely need to call these by name. Just tell Claude what you want to achieve, and it will pick the right tool for the job.

The last think I'll touch on before we wrap up is code connect. Code connect is Figma's way of directly mapping the components you have in Figma, to the components you've implemented in your codebase.

If you want to bridge the gap between design and production entirely, keep an eye on the get_code_connect_suggestions tool. It helps you map Figma components directly to your production code.

Code Connect is the "holy grail" for ensuring the code Claude generates is 100% aligned with your designs.

If you need help setting up Code Connect or integrating AI workflows into your team's design-to-code pipeline, that's exactly what we do at Bitovi. As official Figma partners, we help teams bridge the gap between design and engineering using the latest AI tech.

Check the link in the description to learn more about our AI enablement services.

And if you like this content, we have another video on how to "one-shot" a Jira ticket using Figma and Atlassian's MCPs

Thanks for watching - now go build something cool!