# Haunt Crew App: write-up

## What it does

Haunt injects faults (crashes, timeouts, duplicate calls, rate limits) and load ramps into an agent workflow and grades what happened, using the provider ledger as the truth for side effects (charges, shipments, replies), not the bot's own report. This app puts that into Kiro Crew.

## Parts

| Part | File | Role |
|---|---|---|
| Manifest | `app.json` | Declares the skill, agent, MCP server, page. `network: false`, `storage: true`, requires `node`. |
| Skill | `skills/stress-test-my-workflow/SKILL.md` | Six steps: define, sweep, ramp (on request), report, hand to reviewer, re-run and compare. |
| Agent | `agents/reviewer.json` | Postmortem and hardening spec writer. `fs_write` limited to hardening specs, `.haunt/runs/**` and `.tmp/**`. Cannot change code. |
| MCP | server `haunt` | Tools to define a workflow, run a sweep or ramp, read the report. |
| Page | `page/index.html` | Iframe of the arena UI, accepted only from `http://127.0.0.1:<port>/`. |

## Flow

1. Ask Kiro Crew to stress-test a workflow. The skill calls `haunt_define` with the workflow's `haunt.yaml`.
2. `haunt_sweep` runs one tier of fault cells against local mock servers.
3. `haunt_report` returns the grade and the failing stage.
4. The `reviewer` agent writes a postmortem and a hardening spec with evidence by event sequence number.
5. A developer agent or a person turns the spec into a normal task. Nothing applies a spec automatically.
6. Re-run the sweep and compare grades.

## Limits

The MCP command is relative to the Haunt repository root, so the app needs a Haunt checkout with `npm install` done. The page needs the arena UI running locally (`npm run ui:dev`).
