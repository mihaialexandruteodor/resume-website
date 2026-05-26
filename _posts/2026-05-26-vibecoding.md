---
title: Vibecoding
date: 2026-05-26
categories: [AI]
tags: [ai, quality-of-life]     # TAG names should always be lowercase
---

While I was fairly skeptical about the use of AI agents for programming in the beginning, I have changed my tune once I got to use them at work. 
We are provided a generour Claude subscription, so while at home I use a mix of Claude and Gemini, I'll write these notes for a user that utilizes a paid Anthropic subscrition.

Also, I'll return to this article from time to time to have all notes on this topic in one place.
Without further ado:

## Notes for proper usage of an AI agent for coding
1. Use data banks. An agent is as knowledgeable as its context can reach. With Claude, whether you are using it in an pre-existing codebase or starting from scratch, the `/init` command will generate a `CLAUDE.md` file at the root of the repo. These files are read at the start of each session and feed into the current session context, so setting that up first is a great help.
2. Tell Claude (or other agents) to make notes into the development markdown file or other repo-level note files for each thing that was attempted. As memory might not persist between sessions, this prevents you from going in circles.
3. Use `Plan` and `Act` modes if available. Splitting the session between planning and writing helps with both the human veto process and the context window limitation of current agents.
4. Use `MCP servers` if avaialble. MCP is a standard defined by Anthropic for connecting AI agents with various services, and can be invaluable for gathering knowledge without using more primitive scraping methods.
5. Check if web documentation also provide a markdown version (I already noticed that VISA does this for Cybersource documentation). This will help the agent gather infor without slogging through a less readable format like HTML.
6. Try to set up a shared knowledge database if working with colleagues. An [Obsidian](https://obsidian.md/) vault in a shared folder can be invaluable if multiple people work on the same project.
7. You can ask agents to code review their changes before commiting
8. NEVER allow an agent to execute git commands! This is where I draw the line. Also be careful with database access, there are plenty of horror stories going around!
9. Ask the agent to write unit tests for each new feature developed, or ask it to enhance current tests for fixes or changes.

This is what I have so far, hope it will prove useful. Will update as I find out more!
