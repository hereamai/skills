---
name: hereamai
description: Connect this computer to hereamai (hereamai.com), a live world map of people working with coding agents, so the user shows up on the map while their agent works. Use when the user mentions hereamai, wants to appear on the coding-agent map, share their presence or working status, connect or disconnect this computer, check which hereamai account is connected, update their status by hand, asks how much they used their coding agents this week (tokens, cost, time), asks how many people are working with coding agents right now (on hereamai or around the world), or wants to see, rename or reset the project names in their hereamai session list (e.g. "a session shows up as src"). Runs the `npx hereamai` CLI.
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

## See the user's week

Run when the user asks how much they used their coding agents recently: tokens, estimated cost, time working, sessions, which models. Needs a connected computer.

```bash
npx hereamai@latest week
```

`--period today` (by hour) or `--period 30d` change the range. Show the output to the user as it is, inside a code block. The cost is what the same tokens would cost at public API prices, not a subscription bill; say so if the user asks about money. If it says token counting is off, the user can turn it on at the address it prints.

## See who is working right now

Run when the user asks how many people are working with coding agents right now, or which countries are busiest. It needs no account and sends nothing about the user.

```bash
npx hereamai@latest now
```

It prints the number of people working now, a bar chart of the busiest countries, and the map address. Show the output to the user as it is, inside a code block, so the bars line up. Do not redraw the chart or add names: the CLI shows counts only, and names are visible only on the web map after signing in.

## Show hereamai in Claude Code's status line

Only when the user asks. Works in Claude Code only.

```bash
npx hereamai@latest statusline
```

- If the user has no status line yet, this installs one and you are done. It shows how many people are working now and, when connected, the user's tokens, cost and time today.
- If the user already has a status line, the command changes nothing and prints their current command. Then offer to combine them:
  1. Read the status line command or script it printed (for a script file, open it).
  2. Propose a change that keeps their output and appends hereamai's, e.g. ` · $(node ~/.hereamai/statusline.js < /dev/null)` at the end of their line. Keep `< /dev/null` so it doesn't consume the JSON their script reads from stdin. Show the exact diff.
  3. Edit only after the user says yes. Never replace their status line. Keep a copy of the original script next to it (e.g. `<name>.bak`).
- `node ~/.hereamai/statusline.js` reads only a local cache and prints one line; it is safe to call from another script (with `< /dev/null`). It never sends the status line input (folders, transcript paths).
- To undo: `npx hereamai@latest statusline off` removes only hereamai's own status line. If you merged it into the user's script, remove the line you added.

## Project names

Only if the user turned on "Show project names" in the web settings (Usage tab). Each project keeps the first name it got: the git repository folder, or the working folder when there is no repository. A project outside git that was opened from a subfolder can end up with a name like `src`.

```bash
npx hereamai@latest projects
```

It lists the names on this computer with the agent, the last day used and a short ID. Then:

- Rename: `npx hereamai@latest projects rename <current name> <new name>`. This also renames that project's past sessions on the web, and later signals keep the new name.
- Back to the automatic name: `npx hereamai@latest projects reset <current name>`.
- If several projects have the same name, the command lists them and changes nothing. Ask the user which one, then use its ID (`p_…` from the list) instead of the name, or `--all` if they mean all of them.
- If the name isn't in the list (the project hasn't sent a signal since the CLI was updated), the user can rename it on the web: open the session in https://hereamai.com/me and press "Rename project".
- If it says project names are off, pass on the settings address it prints. Never edit `~/.hereamai/project-names.json` by hand.

## Other commands

| User wants to | Run |
| --- | --- |
| See which account this computer is connected to | `npx hereamai@latest whoami` |
| Show up as working now, with an agent that has no hooks | `npx hereamai@latest status --agent other:<agent-name>` |
| Re-detect agents and install hooks again (also updates the hook script) | `npx hereamai@latest setup` |
| Disconnect this computer (hooks stay installed) | `npx hereamai@latest logout` |
| Remove that status line | `npx hereamai@latest statusline off` |
| Remove the hereamai hooks (and its status line) | `npx hereamai@latest uninstall` |

If a command says the computer was disconnected or is not connected yet, offer to run `login` again.

## What is sent

Hooks send only: the event type (start, activity, heartbeat, stop, end), the agent name, a random project ID, and, only if the user turned them on, token counts per model and the project folder name (one word, fixed per project, shown only to the user in their session list). The project ID is made on this computer from a secret that never leaves it, so the server can't tell which folder it is. Prompts, responses, file paths and code are never sent. The map shows the user's country only, never a city. Sessions, usage and settings are on the web at https://hereamai.com.
