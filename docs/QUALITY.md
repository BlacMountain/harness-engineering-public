# Quality

Quality rules define the minimum verification loop for this harness guide.

MUST:

- Keep `AGENTS.md` short and task-start readable.
- Keep durable rules in `.harness/*.yaml` before explaining them in docs.
- Run `scripts/harness-check` after changing policy, docs structure, or command
  entry points.
- Update docs when policy changes.

SHOULD:

- Convert repeated review comments into policy or executable checks.
- Prefer checks with actionable failure messages.

NEVER:

- Leave durable rules only in chat replies.
- Add a policy that cannot be located from `AGENTS.md`.
