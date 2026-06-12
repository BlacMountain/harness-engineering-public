# Architecture

This repository is a Harness Seed Repository. It does not contain a product
runtime; it bootstraps target-local policy, documentation, and validation entry
points for agent-maintained repositories.

## Layers

- `AGENTS.md`: short startup protocol for bootstrapping target projects.
- `HARNESS_QUICKSTART.md`: project type and workspace decision tree.
- `.harness/`: machine-readable policy source of truth.
- `templates/`: minimal target-local harness templates by project type.
- `docs/`: human-readable guidance, rationale, and governance.
- `scripts/`: executable checks and command entry points.

## Dependency Direction

The root startup protocol routes Agents into the quickstart decision tree, then
into a concrete template. After bootstrap, the target project's local
`AGENTS.md`, `.harness/*.yaml`, and checks become the durable authority.

Policy files define constraints. Documents explain those constraints. Scripts
validate those constraints. Documents and scripts should not introduce rules
that contradict `.harness/*.yaml`.

## Change Rule

When a rule becomes durable, encode it in this order:

1. `.harness/*.yaml`
2. `scripts/harness-check` or another executable check
3. `docs/*.md`
4. `AGENTS.md` only if the rule must be visible at task startup
