# AGENTS.md

This is an infrastructure repository. Before work:

1. Read `.harness/*.yaml`.
2. Confirm repository root and run `git status --short`.
3. Validate infra changes with available lint, plan, or dry-run commands.

Version infrastructure code and templates. NEVER commit secrets, credentials,
state files containing sensitive values, generated plans with secrets, or local
environment files.
