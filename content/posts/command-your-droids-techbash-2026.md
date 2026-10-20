+++
author = "Jonathan Markman"
title = "Command Your Droids - TechBash 2026"
date = "2026-10-19"
description = ""
tags = [
    "Python",
    "CSharp",
    "AI",
    "AgenticDevelopment"
]
+++

I presented "Command Your Droids" at TechBash 2026. [Download the slides from this link](../media/files/command-your-droids.pptx). I recorded the presentation myself here:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/YzQFooFjxxM?si=9eOlvA9SSCmN-S9r" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

This post will fill in a few blanks that I either covered ad-hoc during Q&A or didn't get to address.

---

# The process and tools

The idea of the loop is: 

1. Write a design document outlining the contents of the feature(s)
2. Feed the document into a high reasoning model and task it with creating small tasks on a kanban/task tracking board; have it push back whenever there's ambiguity about the contents of a task or gaps in the implementation
3. Use the software factory script to have a cheaper and more code-focused LLM pick up these focused and atomic tasks one by one and implement them while you do something else

As with any presentation, you don't really get a lot of time to talk about the tools; if you decide to talk about tools, your presentation becomes a boring ramble about tools.

At the client I'm working with, they have an agreement with Cursor[^1] to provide harnesses to the entire company. I personally like Microsoft's offerings and I've had good experiences with Copilot, so I opted to use Copilot for the presentation and for personal work.

I uploaded the associated files to this GitHub repo: https://github.com/jmarkman/LocalSoftwareFactoryScripts

## Kanban

I chose [backlog.md](https://github.com/MrLesk/Backlog.md) for my task tracker. It's a local-first command line tool that tracks tasks, Kanban style (to do, in progress, done), that exposes CLI commands and a MCP server so agents can interact with tasks.

It's a very feature rich tool despite its simplicity: despite primarily relying on a TUI, it exposes a web server and corresponding UI, can group tasks under milestones, can create subtasks under tasks (relationships), and so on.

As I trialed the planning process, I found myself moving from typing in the Cursor IDE to the CLI, then from the CLI to just typing in a text document. I ended up creating a skill, `create-backlog-tasks`, that would handle task creation based on passing my design document/paragraph as a prompt.

Aside from serving as instruction to engage in dialog with me about design or information gaps, the skill resolved some important non-deterministic issues that came up where the LLM of choice would choose a variety of ways to represent the task hierarchy, most notably making subtasks under subtasks.

## Code

Cursor exposes access to their SDK via Python and TypeScript, and I have more familiarity with Python when it comes to using it as a desktop/CLI tool. Copilot's SDK covers more languages, C# and Java being the highlights for corporate environments.

The Python script leverages the Cursor SDK and this was trialed with client work. I originally spent a lot of time messing around with finding the perfect low cost model and jumped between models like GPT 5.4 Mini and Gemini 3.5 Flash, but as experimentation continued, Cursor's in-house Composer 2.5 model became incredibly sufficient for handling each atomic code creation task in the backlog.

[Cursor's documentation](https://cursor.com/docs/sdk/python) is good, but when I was actually implementing the script in Python, I had to leverage the `/sdk` command within the Cursor harness to figure out what the structure of the data I'd be getting back from Cursor was. It was really inconvenient and while I haven't checked the docs again since the script was mostly complete, I hope the docs are a bit more organized and robust.

[Conversely, Microsoft's documentation](https://github.com/github/copilot-sdk/blob/main/docs/getting-started.md) for the Copilot SDK was crystal clear and following along with the examples got me going far quicker than feeling around in the dark with Cursor's `/sdk` command. I ended up using the new [file-based app](https://devblogs.microsoft.com/dotnet/announcing-dotnet-run-app/) system that released with .NET 10 to create a single file C# "script" that acted exactly like the Python script.

## Planning

I spent a chunk of the live demo presenting what the planning experience is like. The LLM portion of planning is a bit new, and during the presentation I ran into some issues where:

- the model wasn't presenting its explanation of what it planned to do with a portion of data modelling clearly
- it kept asking me for permission to execute the backlog.md CLI commands for adding new tasks, which it had never done before

In future experiments, I want to see what will happen if I do batches of features across several planning prompts, versus adding as many of them as I can in one go. The context window didn't seem to be a concern since it never hallucinated anything during the task creation process, but I had to write the aforementioned skill to ensure it followed a specific procedure.

I think it may be worth investigating the "Thought for [x] seconds" reasoning blurbs that the LLMs come up with such that we can tailor task creation skills and rules files to prevent the LLM from going down rabbit holes and refine our prompt usage to ensure LLM pattern matching can translate our ideas correctly.

# Expanding on "Why? Agency"

In the presentation, I emphasized that you never stop writing code yourself, but I would eventually like to write a post about how AI usage can and **should** vary based on experience.

I spoke with one of the software architects at Clear Measure later on at the conference about AI and the software factory. For him, the concept of the factory and his current usage of AI is about bypassing the bottleneck of writing code; he's written systems and code by hand for 20-some-odd years, he's done his reps at the gym.

My general opinion is that with the factory, you don't *have* to stop writing code and depending on your years of experience, it's probably a good idea to keep writing code but use AI as an accelerant for the easy-ish stuff. The only way to get better at something is to do the boring stuff; everything else is entertainment.

For someone like me where I'm just literally years younger with less experience than my colleague, it's still important for me to write code to understand concepts, like why you might write a specific service, when it might be smart to introduce an attribute to the codebase, or when you should probably just use an existing library instead of writing something yourself.


[^1]: They were acquired by SpaceXAI. When I was putting together the presentation, they were still Cursor, and days before the presentation they had agreed to start rebranding to being called Grok Build and being a part of SpaceXAI. We'll know where things stand in a few months, I guess.