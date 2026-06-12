# Docs Structure

Documentation should answer one clear question per location. If a document
answers multiple questions, split it or add an index that routes readers.

## Directory Map

| Location | Question Answered | Contents |
| --- | --- | --- |
| `README.md` | What is this project and how do I use it? | purpose, setup, commands, quick links |
| `AGENTS.md` | What must an Agent do first? | startup protocol, hard rules, verification summary |
| `.harness/*.yaml` | What are the durable machine-readable rules? | project, workspace, git, quality, architecture policy |
| `ARCHITECTURE.md` | How is the system organized? | boundaries, layers, data flow, dependencies |
| `docs/product-specs/` | What should the system be? | product behavior, user workflows, requirements |
| `docs/exec-plans/active/` | How will this task be done now? | task plan, acceptance criteria, steps, risks |
| `docs/exec-plans/completed/` | What was done and what was learned? | completed plans, outcomes, validation notes |
| `docs/exec-plans/tech-debt-tracker.md` | What known debt remains? | debt, owner/status, cleanup path |
| `docs/decisions/` | Why was this choice made? | ADRs, tradeoffs, accepted/rejected options |
| `docs/references/` | What external facts or APIs are we relying on? | links, local summaries, protocol notes |
| `docs/GIT_POLICY.md` | What belongs in version control? | repo type, forbidden files, artifacts, commits |
| `docs/QUALITY.md` | How do we know it is good enough? | checks, coverage expectations, validation |
| `docs/RELIABILITY.md` | How does it fail and recover? | retries, timeouts, observability, rollback |
| `docs/SECURITY.md` | What must stay protected? | secrets, trust boundaries, sensitive data |

## Boundaries

Product specs answer: system should be what.

Architecture answers: system is organized how.

Execution plans answer: this task will be done how.

Decisions answer: this choice was made why.

References answer: this fact came from where.

Policy answers: what rule must be enforced.

## Required Behavior

MUST:

- Keep Agent startup rules in `AGENTS.md`, not buried in docs.
- Keep durable constraints in `.harness/*.yaml`.
- Put one-off task execution details in `docs/exec-plans/active/`.
- Move completed task plans to `docs/exec-plans/completed/`.
- Put external API, paper, platform, or protocol notes in `docs/references/`.
- Put irreversible or high-impact tradeoffs in `docs/decisions/`.

SHOULD:

- Add an index file to any docs directory with more than a few files.
- Link docs from README only when they are useful to humans.
- Turn repeated documentation mistakes into `scripts/harness-check` rules.

NEVER:

- Hide startup rules only in `docs/`.
- Use product specs as task plans.
- Use execution plans as permanent architecture documentation.
- Store secrets, private exports, or large generated artifacts in docs.

## Minimal Docs For New Projects

Every long-lived target project should start with:

- `README.md`
- `AGENTS.md`
- `ARCHITECTURE.md`
- `docs/GIT_POLICY.md`
- `docs/product-specs/index.md`
- `docs/exec-plans/active/`
- `docs/exec-plans/completed/`
- `docs/exec-plans/tech-debt-tracker.md`
- `docs/QUALITY.md`
- `docs/RELIABILITY.md`
- `docs/SECURITY.md`

Add `docs/decisions/` and `docs/references/` when the project starts making
durable tradeoffs or depending on external systems.
