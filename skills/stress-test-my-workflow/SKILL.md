---
name: stress-test-my-workflow
description: Stress-test a workflow with Haunt. Use when the user wants to find how a workflow breaks under faults or load, get a grade, and get a hardening spec.
---

# Stress-test my workflow

Goal: grade the workflow, find the failing stage, hand a hardening spec to a developer agent.

1. Call `haunt_define` with the path to the workflow's `haunt.yaml` (default `apps/haunted-orders/haunt.yaml`). Confirm the stages and targets with the user in one line.
2. Call `haunt_sweep` for one tier, minimum cells, high speed. Run at most one live sweep per session.
3. Call `haunt_ramp` only if the user asks about load. Read the knee and break point from the result.
4. Call `haunt_report` to read the saved report. State the grade and the worst failing stage.
5. Hand over to the `reviewer` agent. It writes `.haunt/runs/<run>/postmortem.md` and `.kiro/specs/hardening-<run>/`.
6. Give the spec to a developer agent. Re-run step 2 and compare grades.

Rules:
- Servers bind 127.0.0.1 only. Stop any server you start.
- No real keys and no external network.
- Fix the workflow, never weaken the grader.
