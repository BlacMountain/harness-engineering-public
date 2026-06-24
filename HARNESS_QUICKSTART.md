# Harness Quickstart

Use this file after `README.md` and `AGENTS.md`. It is the only place in this
seed repository that explains how to apply the harness method to a target
project.

## 1. Confirm Boundaries

Before any code, documentation, or Git work:

1. Identify the workspace boundary.
2. Identify the repository boundary with `git rev-parse --show-toplevel` when
   inside an existing repository.
3. Run `git status --short`.
4. Check whether the target project already has local `AGENTS.md`, docs, and
   validation commands.

If the boundary is unclear, stop and ask for the intended target repository. Do
not run `git init .` by default.

## 2. Ask Before Filling Durable Facts

Do not invent durable project facts. If required content is unclear during
project bootstrap or repair, ask the user before writing it.

| Artifact | Ask when unclear |
| --- | --- |
| `README.md` | Project purpose, setup/run/test commands, and primary workflows. |
| `AGENTS.md` | Agent constraints, verification rules, forbidden operations, and escalation rules. |
| `ARCHITECTURE.md` | System boundary, components, data flow, external services, and durable structure choices. |
| `docs/product-specs/` | Goals, non-goals, users, scenarios, behavior, and acceptance criteria. |
| `docs/exec-plans/` | Draft source, relevant dates, implementation order, risks, rollback, verification commands, and handoff needs. |
| `docs/decisions/` | Final decision, alternatives, constraints, consequences, and status. |
| `docs/references/` | Source, date or version, scope, and limitations. |
| Governance docs | Git and artifact boundaries, quality gates, reliability expectations, and security or trust boundaries. |
| Delivery docs | API docs, user manuals, deployment guides, SDK docs, operator docs, product docs, and the agreed delivery-doc location. |
| `scripts/` | Package manager, runtime, and setup/lint/test/dev command behavior. |

Ask concise questions before writing uncertain facts. Prefer grouped questions
over many small interruptions. Use `Open Questions` only for non-blocking
uncertainties. Stop and ask immediately when the repository boundary, security
boundary, destructive operation, or user-visible acceptance criteria is unclear.

## 3. Identify The Work Stage

| Situation | Next artifact |
| --- | --- |
| Need to define goals, users, behavior, or acceptance | `docs/product-specs/` |
| Need to preserve untriaged draft requirements or external discussion notes | `docs/exec-plans/prepare/` |
| Need to implement a non-trivial change | `docs/exec-plans/active/` |
| Need to record a durable technical or governance choice | `docs/decisions/` |
| Need to rely on an external API, paper, platform, law, or protocol | `docs/references/` |
| Need to create or update project delivery documentation | The target project's agreed delivery-doc location; ask for it when unclear. |
| Need to change repository, artifact, or Git boundaries | `docs/GIT_POLICY.md` |
| Need to change setup, lint, test, dev, or done criteria | `docs/QUALITY.md` and validation scripts |
| Need to change failure, recovery, logs, timeout, or rollback behavior | `docs/RELIABILITY.md` |
| Need to change secret, auth, permission, input, or trust boundaries | `docs/SECURITY.md` |
| Need to enforce a repeated rule mechanically | scripts, lint, tests, hooks, or CI |

Lifecycle docs belong to development management: specs, execution plans,
completed records, decisions, references, quality, reliability, security, and
Git governance. Delivery docs belong to the target project's user- or
operator-facing documentation system: API docs, user manuals, deployment guides,
SDK docs, operator docs, and product docs. Agents may read delivery docs as
project context. When creating or updating delivery docs, use the target
project's agreed documentation location. Development references, such as
third-party API or platform docs used during implementation, route to
`docs/references/`.

Project-cycle records carry lightweight date context. `prepare/` drafts record
the capture date and, when known, the source discussion date. `active/` plans
record the created date and meaningful update dates. `completed/` records
include completion and verification dates. `tech-debt-tracker.md` keeps its
`Date` column as the identified date.

## 4. Target Harness Transfer

For a long-lived target project, create or update only the target-local artifacts
needed by the task:

- `README.md`
- `AGENTS.md`
- `ARCHITECTURE.md`
- `docs/product-specs/index.md`
- `docs/exec-plans/prepare/`
- `docs/exec-plans/active/`
- `docs/exec-plans/completed/`
- `docs/exec-plans/tech-debt-tracker.md`
- `docs/decisions/index.md`
- `docs/references/index.md`
- `docs/GIT_POLICY.md`
- `docs/QUALITY.md`
- `docs/RELIABILITY.md`
- `docs/SECURITY.md`
- `scripts/setup`
- `scripts/lint`
- `scripts/test`
- `scripts/dev` or `scripts/run`

Code projects must use an isolated dependency environment. Python projects
should use `.venv`; other ecosystems should use their normal local isolation.
Pure documentation tasks do not require creating an environment.

Seed repository documents describe this harness project. When applying the
method elsewhere, rewrite project-facing content from the target project's
perspective. Replace seed-specific goals, architecture, commands, validation
rules, artifact boundaries, and lifecycle records with target-local facts. If
the target-local fact is unclear, ask before filling it.

## 5. Transfer Rules

The target project owns its long-term rules. After bootstrap:

1. Read the target project's `AGENTS.md`.
2. Read the target project's task-relevant docs.
3. Run the target project's validation commands.
4. Put durable knowledge and lifecycle records in target-local docs.
5. Put mechanical checks in target-local scripts, lint, tests, hooks, or CI.
6. Stop depending on this seed repository for day-to-day development.
