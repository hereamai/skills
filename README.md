# hereamai skills

I'm here. So is my AI.

[hereamai](https://hereamai.com) is a live world map of people working with coding agents. This repository lets your agent connect your computer to it, and show who is working right now while you wait.

- **Show up on the map.** After you connect once, your country lights up while Claude Code or Codex is working (other agents: by hand with `status`). Only your country is shown, never a city. Prompts, code and file paths are never sent.
- **See who is working now.** Ask your agent "who's coding right now?" and get a bar chart by country. No account needed.

```
15 people working now in 3 countries

  KR  South Korea    10  ████████████████████████
  US  United States   4  ██████████
  JP  Japan           1  ██

Map: https://hereamai.com/map
```

## Install

With [skills.sh](https://skills.sh) (any supported agent):

```bash
npx skills add hereamai/skills
```

As a Claude Code plugin:

```
/plugin marketplace add hereamai/skills
/plugin install hereamai@hereamai
```

Then ask your agent: "connect me to hereamai", or "how many people are coding with an agent right now?".

## Without an agent

```bash
npx hereamai login
```

The CLI is on npm as [`hereamai`](https://www.npmjs.com/package/hereamai).

## License

MIT
