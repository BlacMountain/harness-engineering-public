# AGENTS.md

This is a source repository. Before code work:

1. Read `.harness/*.yaml`.
2. Confirm repository root and run `git status --short`.
3. Use an isolated dependency environment.
4. Run the available setup, lint, test, and dev commands.

MUST keep source, tests, docs, manifests, lock files, and config templates in
git. NEVER commit virtual environments, dependency folders, build output,
coverage, secrets, datasets, checkpoints, or experiment outputs.
