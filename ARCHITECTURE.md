# Architecture

This repository is a knowledge and policy harness. It does not contain a product
runtime; it defines reusable project policy, documentation, and validation
entry points for agent-maintained repositories.

## Layers

- `AGENTS.md`: short agent entry point and required workflow.
- `.harness/`: machine-readable policy source of truth.
- `docs/`: human-readable guidance, rationale, and governance.
- `scripts/`: executable checks and command entry points.

## Dependency Direction

Policy files define constraints. Documents explain those constraints. Scripts
validate those constraints. Documents and scripts should not introduce rules
that contradict `.harness/*.yaml`.

## Change Rule

When a rule becomes durable, encode it in this order:

1. `.harness/*.yaml`
2. `scripts/harness-check` or another executable check
3. `docs/*.md`
4. `AGENTS.md` only if the rule must be visible at task startup
