# Reliability

The harness must make Agent work repeatable.

MUST:

- Provide stable command entry points.
- Fail checks with clear messages and non-zero exit codes.
- Keep generated artifacts out of git.
- Document any manual verification that cannot be automated yet.

SHOULD:

- Keep policy schemas simple enough for Agents and scripts to inspect.
- Add CI once the guide is used by multiple active projects.
