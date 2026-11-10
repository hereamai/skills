---
name: hereamai
description: Connect this computer to hereamai (hereamai.com), a live world map of people working with coding agents, so the user shows up on the map while their agent works. Use when the user mentions hereamai, wants to appear on the coding-agent map, share their presence or working status, connect or disconnect this computer, check which hereamai account is connected, update their status by hand, or asks how many people are working with coding agents right now (on hereamai or around the world). Runs the `npx hereamai` CLI.
---

# hereamai

hereamai shows, on a world map, who is working with a coding agent right now. After the user connects a computer once, the agent's hooks send a small signal while it works. Everything runs through the `hereamai` CLI on npm: always call it as `npx hereamai@latest <command>`.

## Connect this computer

Run only when the user asks to connect, sign up, or appear on the map.

```bash
npx hereamai@latest login
```

- It prints a code like `WDJB-MJHT` and opens `https://hereamai.com/device?code=…` in the browser (or prints the address if it can't).
- **The user approves in the browser.** Tell them to sign in, check that the code on the page matches the one in the terminal, and press **Connect**. Never approve or click on their behalf. If they did not just ask for this, they should close the page.
- The command waits up to 10 minutes for approval. Run it with a long timeout (at least 10 minutes) or in the background, then report what it printed.
- When it succeeds it prints `Connected to @<nickname>`, installs hooks for the agents it finds (Claude Code, Codex), and prints the map address. Tell the user the nickname so they can confirm it is their account.
- First-time users set a nickname and country in the browser before the CLI finishes.
- To name the computer without a prompt: `npx hereamai@latest login --name "<name>"`.
- Codex runs new hooks only after the user trusts them once in `/hooks`. Pass that on if the CLI prints it.

## See who is working right now

Run when the user asks how many people are working with coding agents right now, or which countries are busiest. It needs no account and sends nothing about the user.

```bash
npx hereamai@latest now
```

It prints the number of people working now, a bar chart of the busiest countries, and the map address. Show the output to the user as it is, inside a code block, so the bars line up. Do not redraw the chart or add names: the CLI shows counts only, and names are visible only on the web map after signing in.

## Other commands

| User wants to | Run |
| --- | --- |
| See which account this computer is connected to | `npx hereamai@latest whoami` |
| Show up as working now, with an agent that has no hooks | `npx hereamai@latest status --agent other:<agent-name>` |
| Re-detect agents and install hooks again | `npx hereamai@latest setup` |
| Disconnect this computer (hooks stay installed) | `npx hereamai@latest logout` |
| Remove the hereamai hooks | `npx hereamai@latest uninstall` |

If a command says the computer was disconnected or is not connected yet, offer to run `login` again.

## What is sent

Hooks send only: the event type (start, activity, heartbeat, stop, end), the agent name, the agent's session ID, and, if the user turned it on, token counts per model. Prompts, responses, file paths and code are never sent. The map shows the user's country only, never a city. Sessions, usage and settings are on the web at https://hereamai.com.
