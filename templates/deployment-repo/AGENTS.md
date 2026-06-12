# AGENTS.md

This is a deployment repository. Before work:

1. Read `.harness/*.yaml`.
2. Confirm repository root and run `git status --short`.
3. Validate release and environment changes with dry-run commands when possible.

Version deployment scripts, manifests, and environment templates. NEVER commit
secrets, production credentials, local env files, generated release artifacts,
or machine-specific state.
