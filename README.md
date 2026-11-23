# Haunt Crew App

A Kiro Crew app for [Haunt](https://github.com/kakoritz/Kiroween-Chaos-Official), a stress tester for agent workflows. It runs graded fault sweeps and load ramps against any workflow that has a `haunt.yaml`, shows the run in a haunted arena page, and hands the result to a reviewer agent that writes a postmortem and a hardening spec.

## Features

- MCP server `haunt` with tools `haunt_define`, `haunt_sweep`, `haunt_ramp`, `haunt_report` (and `haunt_badge` in the main repo build).
- Skill `stress-test-my-workflow`: define, sweep, ramp, report, hand over to the reviewer.
- Agent `reviewer`: reads the run log, applies rules R1 to R8, writes `.haunt/runs/<run>/postmortem.md` and `.kiro/specs/hardening-<run>/`. It can write only to those paths and never edits source code.
- Page `/apps/haunt` ("Haunt Arena"): hosts the arena UI from `127.0.0.1` only.
- Offline: no network permission, no real keys, mock servers bind `127.0.0.1`.

See [WRITEUP.md](WRITEUP.md) for how it works.

## Install

The MCP server lives in the Haunt repository, and the manifest command is relative to that repo's root. So install from a Haunt checkout:

```bash
git clone https://github.com/kakoritz/Kiroween-Chaos-Official.git
cd Kiroween-Chaos-Official
npm install
kirocrew app install ./kiro-crew-app
kirocrew app enable haunt
```

This repository is the same app folder, published on its own for browsing and review. Installing a directory is third-party, so Kiro Crew asks you to trust it first. Nothing is enabled automatically.

## How it calls Haunt

`app.json` declares one MCP server:

```json
"haunt": { "command": "node", "args": ["tools/run-ts.mjs", "packages/core/src/bin/haunt-mcp.ts"] }
```

Kiro Crew starts it over stdio. `haunt_define` and `haunt_report` are auto-approved (read only). `haunt_sweep` and `haunt_ramp` ask first because they start local mock servers. The reviewer agent then reads the saved report through `haunt_report`.

## Layout

```
app.json
agents/reviewer.json
skills/stress-test-my-workflow/SKILL.md
page/index.html
```

There are no screenshots in this repository.

## License

MIT, see [LICENSE](LICENSE).
